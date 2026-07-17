---
title: "Week 9"
date: 2026-06-15
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

# Work Log: Web Portal Development – RBAC, JWT Auth, Interactive Maps & LightGBM Dashboard

> **Week 9 - June 15, 2026:** Developed the full-stack enterprise management portal (**Global Fashion Retail Portal**) using Node.js and Express.js. This week covered implementing multi-layer authentication (JWT + TOTP 2FA), designing and enforcing the 7-role RBAC permission matrix, integrating Mapbox GL JS for global store visualization, and connecting the Plotly.js demand forecast dashboard to the AWS Lambda inference API.

---

### Weekly Objectives

- Implement a **multi-mode Data Access Layer** (`db.js`) supporting REST API mode, direct PostgreSQL connection, and local mock JSON fallback.
- Build **JWT token-based session management**: issue signed 8-hour tokens on login, validate them at protected routes via middleware.
- Implement **TOTP Two-Factor Authentication (2FA)** using pure JavaScript HMAC-SHA1 and Base32 algorithms without third-party MFA libraries.
- Design and enforce the **7-role RBAC privilege matrix** controlling dashboard tab visibility and data mutation permissions per business role.
- Integrate **Mapbox GL JS** for 3D global store map rendering and wire the store marker click event to trigger **Plotly.js** forecast chart rendering via the Lambda API.

---

### Daily Activity Log

| Day | Activity | Date |
|-----|----------|------|
| Monday | Built the **Multi-mode Data Access Layer** (`db.js`): implemented the three-mode routing logic (API mode via `API_BASE_URL`, Direct PostgreSQL Pool mode via `DB_HOST`, Mock JSON fallback), tested all three modes against local mock data, and confirmed API mode successfully authenticated with the API Gateway `x-api-key` header. | Jun 15 |
| Tuesday | Implemented **JWT Authentication**: wrote the `/login` POST route issuing `jsonwebtoken`-signed tokens with a 8-hour expiration containing `{ username, role, store_id }` claims, and created Express middleware (`authMiddleware.js`) verifying the Bearer token on every protected API call. | Jun 16 |
| Wednesday | Implemented **TOTP 2FA** (MFA): developed native JavaScript HMAC-SHA1 with Base32 decoding to generate and validate 6-digit OTP codes rotating every 30 seconds. Integrated `otpauth://totp/` QR code generation for compatibility with Google Authenticator. Added MFA event logging (`LOGIN_MFA`, `MFA_ENABLE`, `MFA_DISABLE`) with IP address capture. | Jun 17 |
| Thursday | Designed and enforced the **RBAC Matrix**: implemented role-based route guards (`requirePermission('manage_users')`, `requirePermission('view_transactions')`) across 7 roles (IT Admin, Director, Finance/Auditor, Inventory Manager, Marketing Manager, Store Manager, Sales Staff), confirming each role's sidebar navigation and API endpoint visibility restrictions. | Jun 18 |
| Friday | Integrated **Mapbox GL JS** map and **Plotly.js** forecast dashboard: rendered 3D store markers from database coordinates, implemented the click event handler fetching `/predict` forecasts from the Lambda API via API Gateway, and rendered an interactive Actual vs. Forecasted weekly demand Plotly line chart with inventory shortage alert banners. | Jun 19 |

---

### Key Learning Outcomes

- **Multi-mode Data Abstraction:** Designing the DAL with explicit mode switching (API → PostgreSQL → Mock fallback) enables the same codebase to run in local development, integration testing, and cloud production environments without code changes—only environment variable configuration differs.
- **TOTP Without Dependencies:** Implementing HMAC-SHA1 and Base32 natively (without `speakeasy` or `otplib`) demonstrates that standard RFC 6238 TOTP is achievable in ~50 lines of JavaScript, eliminating an external dependency attack surface.
- **RBAC Middleware Patterns:** Centralizing permission checks in reusable Express middleware (rather than scattering conditional logic in route handlers) makes the permission matrix easily auditable and modifiable from a single file.
- **Mapbox Marker Data Binding:** Populating Mapbox GL JS markers programmatically from a PostgreSQL store list enables the map to reflect live data rather than hardcoded coordinates, making the portal maintainable as the store network grows.
- **Plotly.js Demand Visualization:** Overlaying actual historical quantities against Lambda-generated forecasted quantities in a Plotly line chart provides instant visual validation of the ML model's predictive accuracy to business stakeholders.

---

*Source: [First Cloud Journey - AWS Study Group](https://cloudjourney.awsstudygroup.com/)*
