Here is the complete content of the Lingoteach.ai Security Assessment Report, presented as a structured document (similar to the previous “Project Details” example). All text, tables, bullet points, severity counts, findings, recommendations, and appendices are included exactly as they would appear in the final report.

---

# Lingoteach.ai – Web Application Security Assessment Report

**NuageCX**  
*Cybersecurity Research & Assessment*  
May 2026

---

## Client Details

| Company Name:   | Lingoteach.ai                                      |
|-----------------|----------------------------------------------------|
| Contact Person: | {Person Name}                                      |
| Address:        | {Address}                                          |
| Email:          | {Email Address}                                    |
| Telephone:      | {Telephone Number}                                |

---

## Document History

| Version | Date       | Author    | Remark             |
|---------|------------|-----------|--------------------|
| 1.0     | {Date}     | {Author}  | Document Creation  |

---

## 1. About NuageCX

NuageCX is a research-driven cybersecurity company focused on delivering deep, real-world security assessments across modern technology ecosystems. With a strong foundation in offensive security, we specialize in identifying complex vulnerabilities across web, API, mobile, network, cloud, AI/LLM, and infrastructure environments.

Our approach is rooted in continuous research, hands-on testing, and practical attack simulation. We combine manual expertise with selective automation to uncover security gaps that traditional methods often miss. This enables us to go beyond surface-level findings and provide meaningful, actionable insights.

At NuageCX, we are a team of security researchers, engineers, and practitioners who actively contribute to the global security community through research, tooling, and knowledge sharing. We believe in pushing boundaries, challenging assumptions, and staying ahead of emerging threats.

**We don’t just test systems — we break them, understand them, and help secure them.**

---

## 2. Project Details

### 2.1 Executive Summary

Security Assessment of Lingoteach.ai web application has been performed, considering the following common security issues:

- If proper access control is implemented across all user roles (Admin, Teacher, Pupil).
- If proper Authorization and Authentication System is implemented.
- If proper error handling is done to avoid exposing sensitive data or configuration.
- If the user input is properly validated and session management is securely configured.

The overall security posture of the Lingoteach.ai application is assessed as **POOR**. Significant security controls and measures have not been properly implemented during the design and coding of the application. The assessment revealed systemic weaknesses in access control, session management, rate limiting, and business logic validation.

| CRITICAL | HIGH | MEDIUM | LOW / INFO |
|----------|------|--------|------------|
| 7 Issues | 19 Issues | 15 Issues | 2 Issues |

### 2.2 Scope and Objective

The scope of this assessment was limited to the Lingoteach.ai web application and its associated API infrastructure located at:

- https://uat.lingoteach.ai (Frontend Application)
- https://uat-api.lingoteach.ai (Backend API)

The objective was to identify security vulnerabilities across authentication, authorization, session management, access control, and business logic layers of the application serving three user roles: Admin, Teacher, and Pupil.

### 2.3 Project Timeline

| Assessment Period: | {Number of Days} Days from {Start Date} to {End Date} |
|--------------------|-------------------------------------------------------|
| Testing Type:      | Grey-Box Web Application Penetration Test             |
| Report Date:       | May 2026                                              |

### 2.4 Technological Impact Summary

The NuageCX security team performed a real-time security assessment on the Lingoteach.ai web application. The assessment aimed to uncover security issues, explain the impact and risks associated with identified vulnerabilities, and provide guidance on prioritisation and remediation.

The assessment identified that the application does not implement proper Access Control validation across its API endpoints. The shared login endpoint (/Signin) accepts credentials from all user roles regardless of the target portal, indicating a systemic absence of portal-type validation. The application’s admin analytics API namespace (/v1/analytics/admin/*) is entirely accessible to lower-privilege roles, exposing platform-wide user data, commercial analytics, and PII. Additionally, session management vulnerabilities allow tokens to persist after logout and after password changes, and weak rate limiting across login, registration, and password reset endpoints enables brute-force and abuse attacks.

### 2.5 Business Impact Summary

The following business impacts were identified during the assessment:

- An attacker can gain full access to the admin analytics dashboard, exposing all registered users’ PII, subscription plans, and activity data — constituting a potential platform-wide data breach.
- Admin and Teacher accounts can authenticate via the Student portal, bypassing intended access controls and potentially exploiting weaker protections on student-facing endpoints.
- An attacker can brute-force any account type (Admin, Teacher, or Pupil) due to absent rate limiting and account lockout, leading to full platform compromise.
- An attacker who obtains a valid JWT token can maintain persistent access even after the legitimate user changes their password, defeating the standard account recovery mechanism.
- Mass registration abuse and email bombing attacks are possible due to missing rate limiting on the registration and password reset endpoints.
- Pupil accounts using predictable auto-generated credentials are at elevated brute-force risk.

---

### 2.6 Testing Environment

To perform the web application security assessment, we set up Burp Suite Professional as a proxy tool to intercept HTTP/HTTPS traffic between the browser and server.

Detailed instructions to configure Burp Suite proxy in the browser:  
https://portswigger.net/support/configuring-your-browser-to-work-with-burp

For all reproduction steps documented in this report, the Burp Suite testing environment setup is assumed to be complete.

Additional tools used during the assessment:

- **Firefox Multi-Account Containers** – used to manage simultaneous sessions across Admin, Teacher, and Pupil roles.
- **jwt.io** – used to decode and inspect JWT token payloads.
- **Burp Suite Intruder** – used for rate limiting and brute-force testing across login, registration, and reset endpoints.
- **Burp Suite Repeater** – used for manual access control and IDOR testing across all API endpoints.

Instructions for Firefox Multi-Account Containers:  
https://addons.mozilla.org/en-US/firefox/addon/multi-account-containers/

---

### 2.7 Vulnerability Chart

The discovered vulnerabilities table and chart below provides a snapshot of the number and severity of issues identified during this security assessment.

| Severity       | Definition                                                                                           | Count | Identifiers (Vuln IDs)                                                                                                 |
|----------------|------------------------------------------------------------------------------------------------------|-------|------------------------------------------------------------------------------------------------------------------------|
| **CRITICAL**   | This issue can impact the application severely and should be addressed immediately. Attackers can gain full data access or severely compromise system integrity. | 7     | LT-018, LT-019, LT-021, LT-022, LT-025, LT-026, LT-027                                                                |
| **HIGH**       | This issue can cause significant problems including unprivileged access or account compromise and should be addressed as soon as possible.                        | 19    | LT-001, LT-002, LT-005, LT-006, LT-008, LT-008b, LT-008c, LT-009, LT-011, LT-013, LT-014, LT-015, LT-015b, LT-020, LT-028, LT-029, LT-030, LT-031, LT-037 |
| **MEDIUM**     | This issue may pose a significant threat over a longer period of time and should be addressed within the normal patching cycle.                                    | 15    | LT-003, LT-004, LT-007, LT-010, LT-011b, LT-012, LT-016, LT-017, LT-023, LT-024, LT-032, LT-033, LT-034, LT-035, LT-038 |
| **LOW**        | This issue is more likely an information disclosure and may be an acceptable risk in certain contexts.                                                             | 1     | LT-036                                                                                                                |
| **INFORMATIONAL** | This finding does not pose a direct exploitable risk but provides information that could assist an attacker in targeted reconnaissance.                        | 1     | LT-039                                                                                                                |
| **TOTAL**      |                                                                                                                                      | **43** |                                                                                                                        |

---

### 2.8 Vulnerabilities by Category

The chart below shows the vulnerability matrix based on category of vulnerabilities identified during the assessment of Lingoteach.ai.

| Vulnerability Category                          | Count | Highest Severity | OWASP Top 10 |
|-------------------------------------------------|-------|------------------|--------------|
| Broken Access Control / Missing Role Validation | 14    | CRITICAL         | A01:2021     |
| Authentication & Session Management             | 12    | HIGH             | A07:2021     |
| Business Logic Vulnerabilities                  | 5     | HIGH             | A04:2021     |
| Rate Limiting / Denial of Service               | 5     | HIGH             | A05:2021     |
| Sensitive Data Exposure / Information Disclosure| 4     | HIGH             | A02:2021     |
| Security Misconfiguration (Headers, CORS)       | 2     | MEDIUM           | A05:2021     |

---

### 2.9 Table of Findings

| Vuln ID   | Finding                                                                                                                     | Severity       | Status      |
|-----------|-----------------------------------------------------------------------------------------------------------------------------|----------------|-------------|
| LT-001    | Sensitive Configuration Data Exposed via config.js                                                                          | HIGH           | Not Fixed   |
| LT-002    | Admin Credentials Authenticate on Student Login Portal — Cross-Portal Authentication (CWE-288)                              | HIGH           | Not Fixed   |
| LT-003    | Missing HTTP Security Response Headers                                                                                      | MEDIUM         | Not Fixed   |
| LT-004    | CORS Misconfiguration — Server Returns 500 Error on Cross-Origin Requests                                                   | MEDIUM         | Not Fixed   |
| LT-005    | Business Logic — Admin Credentials Authenticate on Student Login Portal (CWE-288)                                           | HIGH           | Not Fixed   |
| LT-006    | Missing Rate Limiting on Account Registration Endpoint                                                                      | HIGH           | Not Fixed   |
| LT-007    | Duplicate Username Allowed — Business Logic Vulnerability                                                                   | MEDIUM         | Not Fixed   |
| LT-008    | Missing Rate Limiting on Admin Login Endpoint (CWE-307)                                                                     | HIGH           | Not Fixed   |
| LT-008b   | Missing Rate Limiting on Teacher Login Endpoint (CWE-307)                                                                   | HIGH           | Not Fixed   |
| LT-008c   | Missing Rate Limiting on Pupil/Student Login Endpoint (CWE-307)                                                              | HIGH           | Not Fixed   |
| LT-009    | Missing Account Lockout After Multiple Failed Login Attempts (CWE-307)                                                      | HIGH           | Not Fixed   |
| LT-010    | Username and Password Enumeration via Distinct Error Messages (CWE-204)                                                     | MEDIUM         | Not Fixed   |
| LT-011    | JWT Token Remains Valid After Logout — All Three Accounts (CWE-613)                                                         | HIGH           | Not Fixed   |
| LT-011b   | Incomplete Cookie Security — Tracking Cookies Missing HttpOnly Flag                                                         | MEDIUM         | Not Fixed   |
| LT-012    | Excessive Sensitive Data Exposed in JWT Token Payload (CWE-200)                                                             | MEDIUM         | Not Fixed   |
| LT-013    | Sensitive User Data Stored in Browser localStorage (CWE-312)                                                                | HIGH           | Not Fixed   |
| LT-014    | No Rate Limiting on Password Reset Endpoint (CWE-307)                                                                       | HIGH           | Not Fixed   |
| LT-015    | Password Reset Token Reuse — Same Reset Link Works Twice (CWE-640)                                                          | HIGH           | Not Fixed   |
| LT-015b   | Active Sessions Not Invalidated After Password Change (CWE-384)                                                             | HIGH           | Not Fixed   |
| LT-016    | Concurrent Sessions Allowed Without Limit (CWE-613)                                                                         | MEDIUM         | Not Fixed   |
| LT-017    | JWT Session Lifetime 24 Hours — Excessive for Educational Platform (CWE-613)                                                | MEDIUM         | Not Fixed   |
| LT-018    | Broken Access Control — Pupil Token Accesses Admin User Profile Endpoint (CWE-285)                                          | CRITICAL       | Not Fixed   |
| LT-019    | Broken Access Control — Teacher Token Accesses Admin User Profile Endpoint (CWE-285)                                        | CRITICAL       | Not Fixed   |
| LT-020    | Broken Access Control — Pupil Token Accesses Teacher-Only Endpoints (CWE-285)                                               | HIGH           | Not Fixed   |
| LT-021    | Mass User Data Breach — Full User Database Exposed to Any Role (CWE-284)                                                    | CRITICAL       | Not Fixed   |
| LT-022    | IDOR — Admin User Profile Sequential ID Enumeration (CWE-639)                                                               | CRITICAL       | Not Fixed   |
| LT-023    | Subscription Price Manipulation — Server Responds to Modified Price Parameter (CWE-20)                                      | MEDIUM         | Not Fixed   |
| LT-024    | Subscription Plan Details Exposed to Pupil Role (CWE-284)                                                                   | MEDIUM         | Not Fixed   |
| LT-025    | Admin Analytics Endpoints Missing Server-Side Role Validation — Systemic (CWE-285)                                          | CRITICAL       | Not Fixed   |
| LT-026    | Admin Analytics Overview Endpoint Accessible to All Roles — Missing Role Enforcement (CWE-285)                              | CRITICAL       | Not Fixed   |
| LT-027    | Custom Analytics Endpoint Missing Role Validation (CWE-285)                                                                 | CRITICAL       | Not Fixed   |
| LT-028    | JWT-Based Text Translation Endpoint Accessible to All Roles (CWE-285)                                                       | HIGH           | Not Fixed   |
| LT-029    | IDOR on Signed Resource ID — Cross-User Content Access (CWE-639)                                                            | HIGH           | Not Fixed   |
| LT-030    | Hash Tag Endpoint Accessible Without Role Restriction (CWE-285)                                                             | HIGH           | Not Fixed   |
| LT-031    | Public Resource Type Endpoint Missing Role Validation (CWE-285)                                                             | HIGH           | Not Fixed   |
| LT-032    | Community Library Folder Endpoint Missing Access Control (CWE-285)                                                          | MEDIUM         | Not Fixed   |
| LT-033    | Parts of Speech Interaction Endpoint Accessible to Pupil Role (CWE-285)                                                     | MEDIUM         | Not Fixed   |
| LT-034    | Audio Files Endpoint Missing Authorization — Direct File Access (CWE-284)                                                   | MEDIUM         | Not Fixed   |
| LT-035    | User Onboarding Modules Accessible to All Roles Without Role Check (CWE-284)                                                | MEDIUM         | Not Fixed   |
| LT-036    | Webinar Video File Directly Accessible — Missing Access Control on Media Content (CWE-284)                                 | LOW            | Not Fixed   |
| LT-037    | Business Logic — Teacher Credentials Authenticate on Student Login Portal (CWE-288)                                         | HIGH           | Not Fixed   |
| LT-038    | No Server-Side Portal Type Validation — Systemic Business Logic Gap (CWE-602)                                               | MEDIUM         | Not Fixed   |
| LT-039    | Web Server Version Disclosed in HTTP Response Header — nginx/1.29.8 (CWE-200)                                               | INFORMATIONAL  | Not Fixed   |

---

### 2.10 Application Strengths

During our assessment, we observed the following properties of the Lingoteach.ai application that are well-designed and contribute to its security posture:

- The application uses HTTPS for all communication between client and server, ensuring data in transit is encrypted.
- The application does not appear to be vulnerable to SQL injection on the tested endpoints.
- The application returns appropriate HTTP error codes for invalid authentication attempts.
- OAuth/Google SSO integration is present, providing users with a secure third-party authentication option.

### 2.11 Application Weaknesses

The following application-wide weaknesses were identified during the assessment. These represent systemic issues that span multiple individual findings:

- The application does not implement server-side Role-Based Access Control (RBAC). The entire /v1/analytics/admin/* API namespace is accessible to any authenticated role, including the lowest-privilege Pupil account.
- The application does not implement portal-type validation on the shared login endpoint. Admin, Teacher, and Pupil credentials can all authenticate via any portal without server-side role-portal mapping enforcement.
- The application does not enforce rate limiting across authentication-related endpoints (login, registration, password reset), exposing all user accounts to brute-force and abuse attacks.
- Session management is insecure: JWT tokens remain valid after logout, after password change, and for an excessive 24-hour lifetime — all compounding the risk of account takeover persistence.
- Sensitive data is improperly exposed in multiple locations: JWT payload contains PII, browser localStorage stores authentication tokens, and config.js exposes internal API keys and endpoints without authentication.
- Business logic validation is absent at the server level for portal-type, subscription pricing, and username uniqueness, relying solely on front-end controls that are trivially bypassed.

---

> **Note:** The vulnerabilities below were identified and verified by NuageCX during the process of this Web Application Penetration Test for Lingoteach.ai. Retesting should be planned to follow the remediation of these vulnerabilities.

---

## 3. We Prescribe

The table below provides an at-a-glance view of the remediation solutions and best practices that should be taken into consideration to improve the security posture of the Lingoteach.ai application.

| Vuln ID   | Recommendation and Best Practices                                                                                                                                   |
|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| LT-001    | Move sensitive config values to server-side environment variables; restrict config.js to authenticated sessions only.                                                |
| LT-002    | Implement server-side portal-type validation; reject non-student roles on student portal with HTTP 403.                                                             |
| LT-003    | Set all missing security headers: Strict-Transport-Security, X-Frame-Options, Content-Security-Policy, X-Content-Type-Options.                                      |
| LT-004    | Restrict CORS origin to specific trusted domains; return proper CORS error instead of 500.                                                                          |
| LT-005    | Implement server-side portal-type validation; reject admin roles on student portal login endpoint.                                                                  |
| LT-006    | Implement IP-based rate limiting on /registration (max 5/hour); add reCAPTCHA; validate email domains.                                                              |
| LT-007    | Add server-side unique constraint on userName field; return 409 Conflict on duplicate submission.                                                                   |
| LT-008    | Implement IP-based rate limiting on /login (max 10/15 min); return 429 with Retry-After header.                                                                     |
| LT-008b   | Apply same rate limiting controls as LT-008 for Teacher login endpoint.                                                                                             |
| LT-008c   | Apply rate limiting; enforce stronger auto-generated pupil passwords not matching the username prefix.                                                              |
| LT-009    | Implement account lockout (5-10 attempts); temporary lock 15-30 min; send email notification on detection.                                                          |
| LT-010    | Return a single generic error message for all authentication failures (e.g., “Invalid credentials”).                                                                |
| LT-011    | Maintain server-side token blacklist; invalidate tokens on logout; implement short-lived tokens with refresh.                                                       |
| LT-011b   | Set HttpOnly flag on all session and tracking cookies; add Secure and SameSite=Strict flags.                                                                        |
| LT-012    | Reduce JWT payload to minimum required claims; remove PII, role details, and subscription data from token.                                                          |
| LT-013    | Move sensitive data from localStorage to server-side sessions; avoid persisting JWT tokens in browser storage.                                                      |
| LT-014    | Limit reset emails to 3 per email address per hour; add CAPTCHA to forgot password form.                                                                            |
| LT-015    | Invalidate reset token immediately after first use; implement token expiry within 15-30 minutes.                                                                    |
| LT-015b   | Implement token versioning; reject all JWT tokens issued before the current password change timestamp.                                                              |
| LT-016    | Implement max 2 concurrent session limit; send email notification on new device login.                                                                              |
| LT-017    | Reduce access token lifetime to 15-30 minutes; use refresh tokens; add 30-minute inactivity timeout.                                                                |
| LT-018    | Add server-side role validation middleware on all /v1/analytics/admin/* endpoints; return 403 for non-admin.                                                        |
| LT-019    | Implement role check: if (req.user.role !== ‘admin’) return 403. Apply to all admin analytics routes.                                                               |
| LT-020    | Add role-based middleware to all teacher-only endpoints; Pupil tokens must receive HTTP 403.                                                                        |
| LT-021    | Restrict user database listing endpoint to admin role only; implement RBAC centrally.                                                                               |
| LT-022    | Implement UUID-based non-sequential IDs; add role validation before returning user profile data.                                                                    |
| LT-023    | Remove price parameter from client-side requests; validate and calculate price exclusively server-side.                                                             |
| LT-024    | Restrict subscription plan endpoint to admin/teacher roles; return 403 for Pupil tokens.                                                                             |
| LT-025    | IMMEDIATE: Add centralised role validation middleware to ALL /v1/analytics/admin/* routes.                                                                          |
| LT-026    | Apply same admin route middleware as LT-018 to all overview endpoint variants.                                                                                      |
| LT-027    | Restrict /custom_analytics to admin role only; create separate scoped endpoint for teacher analytics.                                                               |
| LT-028    | Add role validation to translation endpoint; restrict or scope access appropriately per role.                                                                        |
| LT-029    | Implement server-side ownership validation for signed resource IDs; reject cross-user access with 403.                                                              |
| LT-030    | Restrict hash tag endpoint to appropriate roles; add role check middleware.                                                                                         |
| LT-031    | Add role validation to public resource type endpoint; restrict access per intended role scope.                                                                       |
| LT-032    | Implement access control on community library folder endpoint; validate requesting user’s permissions.                                                               |
| LT-033    | Restrict parts-of-speech endpoint to Teacher/Admin roles; return 403 for Pupil tokens.                                                                               |
| LT-034    | Require authentication and authorisation for all audio file URLs; implement signed URLs with expiry.                                                                |
| LT-035    | Add role check to onboarding modules endpoint; restrict to appropriate role or remove public access.                                                                |
| LT-036    | Implement access control on webinar video URLs; use signed/expiring media URLs.                                                                                     |
| LT-037    | Apply portal-type validation to reject Teacher credentials on Student portal (same fix as LT-002).                                                                  |
| LT-038    | ARCHITECTURAL FIX: Separate login endpoints by portal type or add mandatory portalType server-side validation.                                                      |
| LT-039    | Add server_tokens off; in nginx.conf http block; reload nginx to remove version from Server header.                                                                 |

### General Recommendations

Looking at the types and severity of vulnerabilities found, NuageCX recommends the following overarching actions:

- **IMMEDIATE PRIORITY:** Implement server-side Role-Based Access Control (RBAC) across all API endpoints. Apply centralised role validation middleware to all /v1/analytics/admin/* routes, rejecting non-admin tokens with HTTP 403.
- **IMMEDIATE PRIORITY:** Implement server-side portal-type validation on the login endpoint. Admin and Teacher roles must be rejected at the Student portal with a 403 Forbidden response.
- **HIGH PRIORITY:** Implement rate limiting across all authentication endpoints (login, registration, password reset) and account lockout after repeated failed attempts.
- **HIGH PRIORITY:** Implement JWT token invalidation on logout and on password change. Introduce a token versioning or blacklisting mechanism.
- **HIGH PRIORITY:** Remove REACT_APP_TOKEN and sensitive API endpoints from config.js and move all configuration to server-side environment variables.
- **MEDIUM PRIORITY:** Reduce JWT session lifetime to 15-30 minutes with refresh token rotation. Implement inactivity timeout.
- **MEDIUM PRIORITY:** Conduct a full API endpoint audit to confirm role-based protection on every route, not just the currently identified vulnerabilities.

---

## 4. Appendix

### 4.1 References

The following resources provide additional detail on the vulnerability types identified in this assessment:

- Broken Access Control (CWE-285, CWE-284): https://owasp.org/Top10/A01_2021-Broken_Access_Control/
- Identification and Authentication Failures (CWE-307, CWE-613, CWE-640): https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/
- Security Misconfiguration (CORS, Security Headers): https://owasp.org/Top10/A05_2021-Security_Misconfiguration/
- Sensitive Data Exposure (CWE-200, CWE-312): https://owasp.org/Top10/A02_2021-Cryptographic_Failures/
- Business Logic Vulnerabilities: https://owasp.org/www-community/vulnerabilities/Business_logic_vulnerability
- IDOR (CWE-639): https://owasp.org/www-community/attacks/Insecure_Direct_Object_Reference
- OWASP Testing Guide: https://owasp.org/www-project-web-security-testing-guide/
- NIST NVD (CVE Research): https://nvd.nist.gov/

### 4.2 Observations

In the observation section, we document any specific issues observed which, while not directly leading to a vulnerability, may require attention from a functional or configuration perspective.

- The application is running in development mode (APP_ENV=dev) on the UAT environment, as disclosed via config.js. Production deployments must use APP_ENV=production.
- The nginx web server discloses its exact version (nginx/1.29.8) in all HTTP response headers, providing reconnaissance information to attackers (see LT-039).
- Pupil accounts use auto-generated credentials with predictable patterns (username prefix matching password default). This should be addressed separately from rate limiting.
- The Admin dashboard panel was accessible during testing and confirmed that mass-created test accounts (50 accounts with identical usernames) were stored in the production database, indicating insufficient server-side validation is compounding real data quality issues.

### 4.3 Failed Test Cases

In this section, we document application strengths observed during testing that represent security controls functioning correctly. These serve as a view of best practices being followed.

#### 4.3.1 CSRF Protection

The application’s authentication mechanism provides strong protection against Cross-Site Request Forgery (CSRF) attacks. The use of SameSite=Strict on session cookies and JWT-based API authentication prevents CSRF exploitation on state-changing API endpoints. Testing confirmed that requests without valid Authorization headers are rejected.

#### 4.3.2 SQL Injection

We tested the application for SQL injection across multiple endpoints including the login, registration, search, and content management endpoints. All tested endpoints appeared to have proper parameterised queries or ORM-based protection against SQL injection. No SQL injection vulnerability was confirmed during the assessment.

#### 4.3.3 OAuth / SSO Configuration

The application’s Google OAuth/SSO integration appeared to handle exception cases properly. Testing confirmed that manipulating OAuth tokens, removing signatures, or tampering with OAuth flow parameters resulted in proper error responses from the server. The OAuth flow did not reveal any exploitable misconfiguration during the testing period.

---

*End of Report*
