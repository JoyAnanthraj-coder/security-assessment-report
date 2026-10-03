Web Application Security Assessment

1. Executive Summary

A security assessment was conducted against an intentionally vulnerable web application deployed within an authorized local training environment. The objective was to identify common web application security weaknesses, evaluate their potential impact, and document appropriate remediation measures.

The assessment focused on authentication, authorization, input validation, session security, and security configuration.

Testing was limited exclusively to the authorized training environment. No third-party systems were targeted.

---

2. Assessment Details

Category| Details
Assessment Type| Web Application Security Assessment
Environment| Authorized Local Training Lab
Target| Intentionally Vulnerable Web Application
Methodology| Manual Security Testing & Observation
Tools| Web Browser, Developer Tools, Burp Suite / Similar Proxy
Date| October 2026

---

3. Objectives

- Identify common web application vulnerabilities.
- Evaluate the effectiveness of authentication and authorization controls.
- Review input validation and output handling.
- Identify insecure configuration practices.
- Assess potential security impact.
- Provide practical remediation recommendations.

---

4. Security Findings

Finding 1 — Broken Object Level Authorization (BOLA)

Severity: High

Affected Component: Object/resource access endpoint

Description

The application did not consistently verify whether the authenticated user was authorized to access a requested resource. This creates a potential horizontal privilege escalation risk where one user may access another user's data.

Security Impact

- Unauthorized access to user data.
- Potential cross-user or cross-tenant data exposure.
- Loss of confidentiality.

Evidence

During authorized testing, changing the identifier associated with a resource request resulted in access to a resource belonging to another test account.

Recommendation

- Perform server-side authorization checks for every object request.
- Verify resource ownership before returning data.
- Implement role- and object-level access controls.
- Deny access by default when authorization cannot be established.
- Log and monitor repeated unauthorized access attempts.

---

Finding 2 — Stored Cross-Site Scripting (XSS)

Severity: Medium

Affected Component: User-generated content / comment functionality

Description

User-controlled content was stored by the application and subsequently rendered to other users without sufficient output encoding or sanitization.

Security Impact

Depending on the application context, successful stored XSS could allow unauthorized actions to be performed within a victim's browser session or alter application content.

Evidence

A harmless test input containing HTML/script markup was stored and subsequently interpreted by the application when the affected page was viewed.

Recommendation

- Apply context-aware output encoding.
- Sanitize user-generated HTML where HTML is intentionally supported.
- Implement a restrictive Content Security Policy (CSP).
- Avoid inserting untrusted content directly into the DOM.
- Use secure framework mechanisms for rendering user input.

---

Finding 3 — Missing or Weak Security Headers

Severity: Low

Affected Component: HTTP response configuration

Description

Several recommended HTTP security headers were missing or insufficiently configured.

Security Impact

Missing security headers can reduce browser-side protections and increase exposure to certain client-side attacks.

Recommendation

Review and configure appropriate headers, including:

- Content-Security-Policy
- Strict-Transport-Security
- X-Content-Type-Options
- Referrer-Policy
- Appropriate frame-ancestors / clickjacking protection

---

Finding 4 — Insecure Session Configuration

Severity: Medium

Affected Component: Session management

Description

Session security controls were reviewed for appropriate cookie attributes and session lifecycle management.

Security Concerns

Potential weaknesses include:

- Missing "Secure" cookie attribute.
- Missing or inappropriate "HttpOnly" configuration.
- Insufficient session expiration.
- Failure to invalidate sessions after logout or security-sensitive changes.

Recommendation

- Use "Secure" and "HttpOnly" cookie attributes where appropriate.
- Configure an appropriate "SameSite" policy.
- Implement server-side session expiration.
- Rotate session identifiers after authentication.
- Invalidate sessions during logout.

---

Finding 5 — Insufficient Input Validation

Severity: Medium

Affected Component: User input fields

Description

Input validation controls were reviewed to determine whether user-supplied data was properly validated before processing.

Security Impact

Insufficient validation can increase the application's exposure to injection and application-logic vulnerabilities.

Recommendation

- Validate input on the server side.
- Use allowlists where practical.
- Apply context-specific output encoding.
- Use parameterized database queries.
- Avoid relying exclusively on client-side validation.

---

5. Risk Summary

Finding| Severity| Primary Risk
Broken Object Level Authorization| High| Unauthorized data access
Stored Cross-Site Scripting| Medium| Client-side code execution
Insecure Session Configuration| Medium| Session compromise
Insufficient Input Validation| Medium| Injection-related risks
Missing Security Headers| Low| Reduced browser protections

---

6. Remediation Priority

High Priority

- Implement object-level authorization checks.
- Verify ownership and permissions on every sensitive resource request.

Medium Priority

- Correct stored XSS through contextual output encoding and sanitization.
- Strengthen session management.
- Improve server-side input validation.

Low Priority

- Review and implement recommended HTTP security headers.

---

7. Conclusion

The assessment identified several common web application security weaknesses within the authorized training environment. The most significant finding involved insufficient object-level authorization, which can result in unauthorized access to resources.

The assessment demonstrates the importance of implementing security controls at the server and data-access layers rather than relying solely on client-side restrictions.

All identified findings should be retested after remediation to confirm that the implemented controls effectively address the identified risks.

---

8. Authorization & Ethical Testing Statement

This assessment was performed exclusively against an intentionally vulnerable application in an authorized local laboratory environment for educational and cybersecurity training purposes.

No third-party systems, accounts, infrastructure, or applications were targeted or tested.
