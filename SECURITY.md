# Security and deployment recommendations

This document lists recommended actions to harden the static landing site when deploying (Netlify, Cloudflare, etc.).

1. HTTP Security Headers (Netlify)
   - Add the `_headers` file (provided) to the site root. Netlify will serve these headers on deploy.
   - Key headers: `Strict-Transport-Security`, `Content-Security-Policy`, `X-Frame-Options`, `X-Content-Type-Options`, `Referrer-Policy`, `Permissions-Policy`.

2. TLS / HTTPS
   - Ensure TLS is enabled (Netlify provides TLS by default). Use HSTS (`_headers` includes it).

3. Web Application Firewall & CDN
   - Consider enabling Cloudflare in front of the site for DDoS mitigation and WAF rules. Configure OWASP rules and rate-limiting for API endpoints.

4. Forms / APIs
   - Validate/sanitize input on the server (Apps Script endpoint). Add rate limits and CAPTCHA for public forms.

5. File Uploads
   - If enabling uploads, store files outside webroot, validate types/sizes, and scan with an antivirus engine (e.g., ClamAV or cloud scanning API).

6. Third-party scripts
   - Minimize third-party scripts. Use Subresource Integrity (SRI) for CDN-hosted scripts when possible.

7. CI / Secrets
   - Do not store secrets in the repo. Use Netlify environment variables or a secret manager. Scan the repo for leaked secrets (git-secrets, truffleHog).

8. Monitoring & Backups
   - Enable logging, uptime monitoring, and regular backups of critical assets and spreadsheet data.

9. Deployment checklist
   - Deploy to a staging site first.
   - Verify CSP does not block essential inline scripts; adjust `_headers` CSP as needed.
   - Test with security scanners (Mozilla Observatory, securityheaders.com).
