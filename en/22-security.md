# 22. Security

### HTTPS on all pages

- Open every page type (homepage, category, product, cart, checkout, account, admin) and verify the browser shows a valid padlock / no mixed-content warning.
- Navigate directly to `http://` versions of each URL and confirm an automatic 301/302 redirect to `https://` occurs.
- Load a page and inspect the browser console for mixed-content warnings (HTTP resources loaded on an HTTPS page).
- Use the Qualys SSL Labs tool (`https://www.ssllabs.com/ssltest/`) against the domain; expect grade A or A+.
- Verify TLS 1.2 and 1.3 are accepted; verify TLS 1.0 and 1.1 are rejected (connection refused or protocol-error).
- Check the SSL certificate expiry date is not within 30 days of the test date.
- Confirm the certificate covers all sub-domains in use (wildcard or SANs).

### HSTS header

- Make a request with `curl -I https://example.com` and confirm the response contains `Strict-Transport-Security` with a `max-age` of at least 31536000 (one year).
- Verify `includeSubDomains` is present in the HSTS header value.
- Verify `preload` is present if the domain is intended for HSTS preload list submission.
- Confirm that the HSTS header is NOT served over plain HTTP (it would be ignored and is a misconfiguration).
- Use OWASP ZAP passive scan to flag any missing or weak HSTS configuration automatically.

### Content Security Policy

- Fetch any page with `curl -I` and confirm a `Content-Security-Policy` (or `Content-Security-Policy-Report-Only`) header is present.
- Verify the policy does not include `unsafe-inline` for scripts without a nonce or hash — inject `<script>alert(1)</script>` into a URL parameter to confirm it is blocked by CSP before reaching the DOM.
- Verify the policy does not include `unsafe-eval`.
- Confirm `default-src` or `script-src` does not include the wildcard `*`.
- Check that an external CDN or analytics domain (e.g., `www.google-analytics.com`) required by the application is explicitly whitelisted; loading should succeed.
- Use the online CSP Evaluator (`https://csp-evaluator.withgoogle.com/`) or OWASP ZAP active scan to flag policy weaknesses.
- Attempt to load an inline event handler: `<img src=x onerror=alert(1)>` as a URL parameter; confirm it is blocked.
- Confirm `frame-ancestors` is set to prevent clickjacking (or that an `X-Frame-Options` header is present as fallback).

### XSS protection (input fields, URL parameters)

- In every visible input field (search, name, address, review, contact form, coupon code) submit the payload `<script>alert('XSS')</script>` and confirm no alert fires and the payload is either escaped or stripped in the output.
- Test reflected XSS via URL parameters: navigate to `https://example.com/search?q=<script>alert(1)</script>` and confirm the query is rendered as plain text, not executed.
- Test stored XSS: submit `<img src=x onerror=alert(1)>` as a product review or account name; then view the page that renders that content and confirm no alert fires.
- Test DOM-based XSS by injecting `#<img src=x onerror=alert(1)>` in the fragment and confirm no execution.
- Use Burp Suite's active scan (Intruder or Scanner) against all form endpoints with a polyglot XSS payload list.
- Confirm that output encoding is applied in HTML context, attribute context, and JavaScript context separately.
- Verify the `X-XSS-Protection: 1; mode=block` header is present (legacy browsers) or that CSP alone covers modern browsers.
- Test XSS via HTTP headers that may be reflected: submit a malicious `Referer` or `User-Agent` header and inspect rendered admin pages.

### SQL injection protection

- In every search field and URL parameter that queries the database, submit `' OR 1=1--` and confirm the response does not return an unexpected data set or error message.
- Submit `'; DROP TABLE users;--` in name and address fields at checkout and confirm no database error occurs and the application continues to function.
- Use a time-based blind SQLi payload: `' OR SLEEP(5)--` in a search field; confirm the response time does not increase by ~5 seconds.
- Use `sqlmap` in safe mode (`sqlmap -u "https://example.com/search?q=test" --level=2 --risk=1`) against all parameterised endpoints and confirm no injectable parameters are found.
- Submit `1' AND '1'='1` as a product ID in the URL (`/product/1' AND '1'='1`) and confirm a 404 or sanitised error is returned rather than a valid product page.
- Use Burp Suite's active scanner against all form submissions and API endpoints.
- Confirm that ORM/prepared statements are used: review a representative sample of database-calling code paths to verify parameterised queries, not raw string concatenation.

### CSRF protection (forms)

- Open the browser developer tools, submit any state-changing form (add to cart, place order, change password, update address), and intercept the request; confirm a CSRF token is present in the request body or a custom header.
- Replay the same request with the CSRF token removed; confirm the server returns a 403 or equivalent rejection.
- Replay the request with an invalid/forged CSRF token (`X-CSRF-Token: INVALID`); confirm rejection.
- Craft an external HTML page with a hidden form that POSTs to the shop's order endpoint, open it in the same browser where you are logged in, and confirm the request is rejected.
- Confirm that AJAX requests use either the double-submit cookie pattern or a custom request header (`X-Requested-With: XMLHttpRequest`) and that the server validates it.
- Use OWASP ZAP's active scan to automatically flag forms lacking CSRF protection.
- Confirm that logout also uses a POST request protected by CSRF (a GET logout is a CSRF vulnerability).

### Brute force protection (login)

- Attempt to log in with the correct email address and 10 consecutive wrong passwords; confirm the account is temporarily locked or a CAPTCHA is presented.
- Confirm the lockout/CAPTCHA threshold is no more than 5–10 failed attempts.
- After triggering a lockout, attempt to log in immediately with the correct password; confirm access is denied for the lockout duration.
- Test distributed brute force: send login attempts from different IP addresses (use Burp Suite with `X-Forwarded-For` header manipulation) and confirm that server-side rate limiting is not bypassed purely by IP rotation.
- Verify that the error message for a failed login is generic ("Invalid email or password") and does not distinguish between "email not found" and "wrong password" (user enumeration prevention).
- Confirm that "Forgot password" endpoint is also rate-limited (submit 20 reset requests for the same email within one minute).
- Check that password reset tokens are single-use and expire within a reasonable time (e.g., 1 hour).

### Rate limiting (API, login)

- Send 100 rapid GET requests to a public API endpoint (`/api/products`) using `ab -n 100 -c 10` or Burp Suite Intruder; confirm a 429 Too Many Requests response is returned before the 100th request.
- Send 50 rapid POST requests to `/api/checkout` or the add-to-cart endpoint; confirm rate limiting activates.
- Confirm the response includes a `Retry-After` header or `X-RateLimit-Remaining` header.
- Attempt to bypass rate limiting by rotating the `X-Forwarded-For` header value; confirm the server uses a combination of session/account identity and IP, not IP alone.
- Send 20 search requests per second and confirm the server does not return 500 errors but instead gracefully returns 429 responses.
- Verify that rate limit responses do not leak internal server information in the response body.

### Secure password hashing

- Register an account, then access the database directly (or via admin export if available) and confirm the stored password is a bcrypt, Argon2id, or scrypt hash (not MD5, SHA-1, or plain text).
- Confirm the hash has a cost factor appropriate for the algorithm (bcrypt cost ≥ 10; Argon2id with adequate memory parameter).
- Confirm that a rainbow-table attack on the hash would be infeasible by verifying a per-user salt is incorporated (bcrypt and Argon2 include this automatically).
- Submit a password reset and confirm the reset token stored in the database is also hashed, not stored in plain text.
- Attempt to log in with the literal bcrypt hash string as the password; confirm it is rejected.

### Session management (timeout, fixation)

- Log in, then remain idle for the configured session timeout period; attempt to access an authenticated page and confirm redirection to login.
- Log in, copy the session cookie value, log out, and attempt to use the copied cookie in a new request; confirm the session is invalidated after logout.
- Before logging in, capture the session ID cookie assigned to a guest; log in; confirm the session ID changes after successful authentication (session fixation prevention).
- Attempt session fixation: set a known session ID in a cookie before visiting the login page, log in, and confirm the server issues a new session ID rather than accepting the attacker-controlled one.
- Open two browser tabs, log out in one tab, then try to perform an action in the second tab; confirm the session is no longer valid in the second tab.
- Confirm that concurrent sessions from multiple devices can be managed or limited based on the application's policy.
- Use OWASP ZAP's session management scan to automatically detect weaknesses.

### Cookie flags (Secure, HttpOnly, SameSite)

- Use browser developer tools (Application > Cookies) or `curl -v` to inspect every cookie set by the application; confirm each session/auth cookie has the `Secure` flag.
- Confirm every session/auth cookie has the `HttpOnly` flag (preventing JavaScript access via `document.cookie`).
- Confirm every session/auth cookie has `SameSite=Lax` or `SameSite=Strict` (not `SameSite=None` without `Secure`, and not absent).
- Attempt to read a session cookie via JavaScript in the browser console (`document.cookie`); confirm the session cookie does not appear in the output due to `HttpOnly`.
- Confirm cookies are not set with excessively long expiry dates for session cookies (they should be session-scoped or expire within a reasonable period).
- Use OWASP ZAP's passive scan to automatically flag cookies missing security flags.

### File upload validation

- Attempt to upload a PHP webshell (`shell.php` containing `<?php system($_GET['cmd']); ?>`) disguised as an image by renaming it `shell.jpg`; confirm it is rejected or stored in a non-executable location.
- Attempt to upload a file with a double extension: `malicious.php.jpg`; confirm the server does not execute it.
- Upload a valid image but with a malicious SVG payload (`<svg><script>alert(1)</script></svg>`) as the file content; confirm XSS does not execute when the file is viewed.
- Upload a file exceeding the maximum allowed size; confirm it is rejected with an appropriate error message.
- Upload a file type not in the allowed list (e.g., `.exe`, `.sh`, `.bat`); confirm rejection.
- Verify that uploaded files are served from a separate domain or subdomain (or with `Content-Disposition: attachment`) to prevent stored XSS from SVG/HTML uploads.
- Confirm that uploaded filenames are sanitised or replaced with random strings to prevent directory traversal via filename (`../../etc/passwd`).
- Attempt a zip bomb upload (if zip files are accepted); confirm the application applies size limits before decompression.

### Admin area secured

- Attempt to access the admin URL (`/admin`, `/backend`, `/dashboard`, `/wp-admin` as common guesses) without being logged in; confirm a 401/403 or login redirect is returned, not the admin page.
- Attempt to access admin URLs directly after logging in as a regular customer account; confirm a 403 Forbidden is returned.
- Attempt to access specific admin API endpoints (e.g., `GET /api/admin/orders`) with a customer-role JWT or session token; confirm 403.
- Confirm the admin login form is protected by brute force / rate limiting independently of the customer login.
- Confirm the admin area is restricted to specific IP ranges if the application supports it (test from an unauthorised IP if feasible in the test environment).
- Attempt to use an IDOR attack: as a logged-in customer, modify the user ID parameter in an account API request to another user's ID (`/api/account/42` when logged in as user 99); confirm 403 or that only your own data is returned.
- Confirm admin sessions expire after inactivity.
- Verify that admin user roles/permissions are enforced server-side, not only hidden in the UI.

### PCI-DSS compliance (payment data)

- Complete a test purchase with a Stripe test card and confirm that full card numbers are never present in server-side logs, database tables, or HTTP responses.
- Inspect the page source and network requests during card entry; confirm raw card numbers are never transmitted to the shop's own server (Stripe Elements / tokenisation must handle this).
- Confirm that the CVV is never stored anywhere, even temporarily.
- Verify that the payment form is loaded from the payment provider's domain (iframe) and is not a custom-built HTML form that POSTs card data to the shop's own server.
- Use Burp Suite to inspect all network requests during the payment flow and confirm no request to the shop's domain contains raw PAN or CVV.
- Confirm that payment-related endpoints use TLS 1.2+ and that the certificate is valid.
- Confirm 3D Secure / Strong Customer Authentication is triggered for transactions that require it under PSD2 (test with Stripe's `4000002500003155` test card).

### No sensitive data in URLs

- Complete the full checkout and order confirmation flow; inspect browser address bar and server-side access logs for the presence of order IDs, email addresses, session tokens, or payment references in query strings.
- Trigger a password reset and confirm the reset token appears only in the email link, not in a URL that is also logged by analytics scripts.
- Log in and navigate to the account area; confirm the session token is stored in a cookie, not appended to the URL as `?session=...`.
- Share a product URL with a friend; confirm it does not contain any personally identifiable information or user-specific token.
- Inspect referrer headers on outbound links (to external payment providers, social media): confirm sensitive path segments are not leaked via the `Referer` header (set `Referrer-Policy: no-referrer` or `strict-origin` on sensitive pages).
- Confirm that order confirmation URLs shared via email do not expose other customers' data through predictable / sequential order IDs without an additional secret token.

### Error pages without sensitive info (no stack trace)

- Trigger a 404 error by visiting `/nonexistent-page-xyz`; confirm the error page does not display file paths, database connection strings, framework version, or server software version.
- Trigger a 500 error (e.g., by submitting a malformed API request, or by temporarily introducing a configuration error in the test environment); confirm the error page shows only a user-friendly message.
- Submit invalid data types to API endpoints (e.g., a string where an integer is expected) and confirm the JSON error response does not include a stack trace or internal variable names.
- Confirm that PHP/framework debug mode (`APP_DEBUG=true`, `WP_DEBUG=true`, etc.) is disabled in production; check by looking for stack traces or variable dumps in any error response.
- Confirm the HTTP `Server` response header does not reveal the exact server software version (e.g., `Apache/2.4.54` should be suppressed or genericised).
- Confirm the `X-Powered-By` response header is absent or suppressed (it should not reveal `PHP/8.1.x`).
- Use OWASP ZAP's passive scan to flag any responses that contain stack traces or internal path information.

### Dependency scanning (known vulnerabilities)

- Run `composer audit` (PHP) or `npm audit` (Node.js) in the project's CI/CD pipeline and confirm there are no known critical or high-severity vulnerabilities in direct or transitive dependencies.
- Use the OWASP Dependency-Check tool (`dependency-check --scan ./`) against the project directory; confirm the HTML report contains no CVSS-high or CVSS-critical findings that are unmitigated.
- Run Snyk (`snyk test`) or GitHub Dependabot against the repository; confirm no unresolved critical alerts are present.
- Verify that outdated dependencies identified during scanning have an upgrade path or an accepted risk waiver documented.
- Confirm that the process is automated: dependency scanning runs on every pull request and at least weekly on the main branch.
- Check that third-party JavaScript libraries loaded from CDN use Subresource Integrity (SRI) hashes (`integrity="sha384-..."`) so that a compromised CDN cannot inject malicious code.

### Robots.txt does not reveal admin URLs

- Fetch `/robots.txt` and inspect all `Disallow` directives; confirm that none of them reveal previously unknown admin, configuration, backup, or API paths that should be kept secret.
- Confirm that paths listed in `robots.txt` are still access-controlled: visit each disallowed path without authentication and confirm a 401/403 response, not the actual content.
- Confirm that sensitive paths (e.g., `/backup/`, `/.env`, `/config/`, `/phpmyadmin/`) are not listed in `robots.txt` as a form of security through obscurity — they should be access-controlled, not merely hidden from crawlers.
- Verify that `robots.txt` itself is publicly accessible (HTTP 200) but does not itself contain credentials, API keys, or internal hostnames.
- Use a tool such as `gobuster` or `dirb` against the test environment to enumerate common paths; confirm that none of the discovered paths return sensitive data and that any discovered admin paths require authentication.
