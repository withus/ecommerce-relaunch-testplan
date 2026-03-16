# 28. Infrastructure / DevOps

### SSL Certificate Valid

- Verify certificate is not expired: `openssl s_client -connect yourdomain.com:443 -servername yourdomain.com 2>/dev/null | openssl x509 -noout -dates`
- Confirm issuer is a trusted CA (Let's Encrypt, DigiCert, etc.): `openssl s_client -connect yourdomain.com:443 2>/dev/null | openssl x509 -noout -issuer`
- Verify the certificate covers the correct domain(s) including `www` subdomain: `openssl s_client -connect yourdomain.com:443 2>/dev/null | openssl x509 -noout -subject -ext subjectAltName`
- Check certificate chain is complete (no intermediate cert missing): `openssl s_client -connect yourdomain.com:443 2>/dev/null | grep -A2 "Certificate chain"`
- Verify certificate expiry is at least 30 days away (set calendar reminder for renewal 60 days before expiry)
- Confirm HTTPS redirect is active — HTTP must redirect to HTTPS: `curl -I http://yourdomain.com` — expect `301` or `302` with `Location: https://`
- Verify HSTS header is present and has a `max-age` of at least 1 year: `curl -sI https://yourdomain.com | grep -i strict-transport`
- Test that mixed content does not exist — all subresources (images, scripts, styles) must be loaded over HTTPS: check browser console for mixed content warnings
- Test SSL rating using external tool: `curl "https://api.ssllabs.com/api/v3/analyze?host=yourdomain.com&publish=off&all=done"` or visit https://www.ssllabs.com/ssltest/ — target grade A or A+
- Confirm TLS 1.2 and TLS 1.3 are supported; TLS 1.0 and 1.1 must be disabled: `openssl s_client -connect yourdomain.com:443 -tls1 2>&1 | grep -i "handshake failure"`
- Verify the certificate serial and fingerprint match across staging and live environments after deployment

### DNS Correctly Configured

- Verify A record points to the correct server IP: `dig A yourdomain.com +short`
- Verify `www` CNAME or A record resolves correctly: `dig A www.yourdomain.com +short`
- Confirm no unexpected or stale DNS records remain (e.g., old server IPs): `dig ANY yourdomain.com`
- Verify MX records are correctly set for transactional email delivery: `dig MX yourdomain.com +short`
- Verify SPF record exists and is correct: `dig TXT yourdomain.com +short | grep spf`
- Verify DKIM record exists for the sending domain: `dig TXT default._domainkey.yourdomain.com +short`
- Verify DMARC record exists and policy is set to at least `p=quarantine` or `p=reject`: `dig TXT _dmarc.yourdomain.com +short`
- Check DNS propagation from multiple geographic locations using https://dnschecker.org or `dig @8.8.8.8 yourdomain.com` vs. `dig @1.1.1.1 yourdomain.com`
- Verify TTL values are reasonable (low TTL during migration, higher TTL after stable): `dig A yourdomain.com | grep -i ttl`
- Confirm CAA records restrict certificate issuance to the intended CA: `dig CAA yourdomain.com +short`
- Test that the domain does not resolve to the old server after migration cutover
- Verify DNSSEC is enabled and validates correctly (if used): `dig A yourdomain.com +dnssec`

### Server Monitoring

- Confirm an uptime monitoring service is active and alerting (e.g., UptimeRobot, Pingdom, Datadog): verify alert email/SMS is received within 2 minutes of simulated downtime
- Verify HTTP 200 response is being monitored at minimum on the homepage, checkout, and a product page: `curl -o /dev/null -s -w "%{http_code}" https://yourdomain.com`
- Confirm CPU and RAM usage alerts are configured with thresholds (e.g., alert at 80% CPU for 5 minutes)
- Verify disk space monitoring is active with alert at 80% full: `df -h /` — confirm monitoring tool reflects the same values
- Confirm database connection pool monitoring is in place — alert if pool exhaustion occurs
- Verify response time monitoring is active and alerting if average TTFB exceeds 2 seconds
- Test that on-call notifications are routed correctly (correct team member receives alert)
- Confirm monitoring dashboard is accessible and shows current server health metrics
- Verify that third-party dependencies (payment gateways, shipping APIs) are monitored via synthetic checks
- Test monitoring after deployment — verify no false positives triggered by deployment restart
- Confirm SSL certificate expiry is monitored with a 30-day advance alert

### Error Logging (Sentry, Bugsnag)

- Verify Sentry/Bugsnag DSN is correctly configured in production environment (not pointing to staging project)
- Trigger a known test error and confirm it appears in the error tracking dashboard within 60 seconds
- Verify that 5xx server errors are captured and assigned to the correct project
- Confirm that JavaScript frontend errors are also captured (check browser console error visibility in dashboard)
- Verify that error grouping is working — the same recurring error should not create duplicate noise
- Confirm that sensitive data (passwords, credit card numbers, full session tokens) is NOT included in error payloads — check source context in dashboard
- Verify that the correct environment tag (`production` vs. `staging`) is set on all events
- Confirm alerts are configured: immediate notification for new high-priority errors, daily digest for low-priority
- Verify that deploy tracking is configured — new releases in Sentry/Bugsnag should correspond to code deployments
- Test that unhandled promise rejections and async errors are captured in frontend JS tracking
- Confirm that PHP fatal errors and exceptions all route to the error tracker, not just to the server log file
- Verify error rate thresholds: alert if error rate exceeds X events/minute (define threshold per project)

### Backup Strategy

- Confirm automated database backups run daily and are stored off-server (S3, remote storage): verify by checking backup storage timestamps
- Verify file system backups (uploads, media, config) are included in the backup schedule
- Test backup restoration in a staging environment — restore the latest backup and verify the site is functional
- Confirm backup retention policy: at minimum 7 daily, 4 weekly, 3 monthly backups retained
- Verify backup integrity by testing restore of a randomly selected older backup (not just the latest)
- Confirm backups are encrypted at rest: verify encryption flag on S3 bucket or backup tool config
- Test that a full disaster recovery restore can be completed within the defined RTO (Recovery Time Objective)
- Verify that backup alerts fire if a scheduled backup fails to complete
- Confirm database backups are consistent (no partial writes) — use `mysqldump --single-transaction` for MySQL or equivalent
- Verify that the backup process does not impact production performance during execution (run during off-peak hours)
- Document and verify the restoration procedure is accessible to all team members (not dependent on a single person)

### Deployment Pipeline

- Verify that deployment is automated and does not require manual file copying to the production server
- Confirm that the pipeline runs automated tests before deploying to production (unit tests, smoke tests)
- Verify that deployment to production requires explicit approval or a protected branch merge (no accidental auto-deploy from any branch)
- Confirm that the pipeline performs a health check after deployment and rolls back automatically on failure
- Verify that environment-specific configuration (`.env`, secrets) is injected via CI/CD variables, not committed to the repository
- Confirm that deployment duration is measured and acceptable (target: under 5 minutes for a standard deploy)
- Verify that zero-downtime deployment is in place (blue-green, rolling, or symlink-swap strategy) — test by monitoring uptime during a deployment
- Confirm that the deployment log is stored and accessible for post-mortem review
- Verify that database migrations run automatically during deployment and are idempotent
- Test that a failed migration causes the deployment to halt and not leave the application in a broken state
- Confirm that static assets are versioned/cache-busted on each deployment so users receive updated CSS/JS

### Staging Environment Available

- Verify staging URL is accessible and password-protected (not publicly indexed): `curl -sI https://staging.yourdomain.com` — expect `401` or basic auth prompt
- Confirm staging is excluded from search engine indexing: `curl -s https://staging.yourdomain.com/robots.txt | grep -i disallow`
- Verify staging environment uses a separate database from production (no risk of corrupting live data)
- Confirm staging uses production-equivalent configuration (same PHP version, same server software, same Redis/queue setup)
- Verify that payment gateways in staging use sandbox/test mode credentials — no real charges should be possible
- Confirm that transactional emails in staging are redirected to a test mailbox (e.g., Mailtrap) and NOT sent to real customers
- Verify that staging can be refreshed from a production database dump for realistic testing
- Confirm that the staging deployment pipeline mirrors the production pipeline (same steps, same scripts)
- Test that all integrations (shipping API, ERP, analytics) are configured with staging/sandbox endpoints in the staging environment
- Verify that access to staging is restricted to the development team (IP whitelist or HTTP basic auth)

### Rollback Capability

- Verify a documented rollback procedure exists and is accessible to all team members
- Confirm that the previous deployment artifact (or Docker image) is retained and can be redeployed within 5 minutes
- Test a rollback by deploying a known-good previous version to staging and verifying it works correctly
- Verify that database rollback is addressed in the rollback plan — confirm which migrations are reversible and which require a database restore
- Confirm that rollback can be triggered without requiring access to the original developer (bus factor test)
- Verify that after rollback, the health check passes and no error spike appears in Sentry/Bugsnag
- Test that a rollback during peak traffic does not cause data loss for in-flight orders (checkout sessions, pending payments)
- Confirm that the rollback process is automated where possible (one command or one button in CI/CD dashboard)
- Verify that rollback duration is measured and within the defined RTO

### Auto-Scaling (During Traffic Spikes)

- Verify auto-scaling configuration is enabled in the cloud provider dashboard (AWS Auto Scaling, DigitalOcean Droplet resize, etc.)
- Confirm that scale-out triggers are set at appropriate CPU/memory thresholds (e.g., scale out when CPU > 70% for 3 minutes)
- Test auto-scaling behavior by simulating a traffic spike with a load testing tool: `ab -n 5000 -c 200 https://yourdomain.com/` and monitor whether new instances are provisioned
- Verify that new instances pass the health check and receive traffic within the defined scale-out time (target: under 3 minutes)
- Confirm that scale-in (removal of excess instances) occurs after traffic drops to avoid unnecessary cost
- Verify that session persistence is handled correctly when multiple instances are running (sessions stored in Redis/database, not on local disk)
- Confirm that the database tier is not the bottleneck during scale-out — verify connection pool limits are sufficient for multiple app instances
- Verify that auto-scaling events are logged and trigger notifications to the operations team
- Test that scheduled scaling rules exist for known high-traffic events (newsletter campaigns, Black Friday) — verify pre-warming is possible
- Confirm that the CDN absorbs static asset requests so auto-scaling applies only to dynamic application traffic

### Cron Jobs / Scheduled Tasks

- List all configured cron jobs and verify each one is documented with its schedule, purpose, and expected output: `crontab -l` or check CI/CD scheduler
- Verify that each cron job runs successfully in the production environment by checking its last execution timestamp in the application log or database
- Confirm that cron jobs do not run multiple overlapping instances simultaneously (use a mutex/lock mechanism)
- Verify that failed cron jobs generate an alert (email, Sentry, monitoring system)
- Test critical cron jobs manually by triggering them on staging and verifying the expected outcome (e.g., abandoned cart email sent, inventory sync completed)
- Confirm that cron job output is logged and retained for at least 7 days for debugging purposes
- Verify that timezone configuration is correct for time-sensitive cron jobs (e.g., daily report at 08:00 local time, not UTC)
- Confirm that cron jobs are resilient to missing dependencies — a cron that runs against a temporarily unavailable external API should retry or log gracefully, not crash silently
- Verify that database-heavy cron jobs (e.g., report generation) are scheduled during off-peak hours
- Test that disabling a cron job on one server does not break functionality if it is expected to run on a primary server in a multi-server setup

### Queue System (Emails, Jobs)

- Verify that the queue worker process is running and monitored: `ps aux | grep queue:work` (Laravel) or equivalent
- Confirm that queue worker crashes trigger an automatic restart (via Supervisor, systemd, or process manager)
- Verify that failed jobs are captured in a failed jobs table/dead letter queue and generate an alert
- Test end-to-end: place a test order and verify that the order confirmation email is dispatched via the queue and delivered within 60 seconds
- Confirm that queue backlog monitoring is in place — alert if the queue depth exceeds a defined threshold (e.g., 500 pending jobs)
- Verify that queue workers are scaled appropriately (multiple workers for high-priority queues like payment processing vs. low-priority queues like reports)
- Test that retrying a failed job from the dead letter queue works correctly without causing duplicate side effects (idempotency)
- Confirm that long-running jobs have appropriate timeouts configured and do not block the queue indefinitely
- Verify that queue connection credentials (Redis, database, SQS) are stored securely and not hardcoded
- Test queue behavior under load: simulate 1,000 queued jobs and verify all are processed within an acceptable time window
- Confirm that job processing is logged with enough detail to diagnose failures (job class, payload summary, error message, stack trace)

### Redis / Cache Layer

- Verify Redis is running and reachable: `redis-cli -h localhost ping` — expect `PONG`
- Confirm Redis is used for session storage (not file-based sessions in production): check application config and verify session keys in Redis: `redis-cli keys "laravel_session:*" | head -5`
- Verify Redis is used for application caching (page fragments, query results): check cache keys: `redis-cli keys "*" | head -20`
- Confirm that cache invalidation works correctly after product/price updates — update a product price and verify the cached page reflects the change within the configured TTL
- Verify Redis memory usage is within acceptable limits and eviction policy is set appropriately: `redis-cli info memory | grep used_memory_human` and `redis-cli config get maxmemory-policy`
- Test that the application degrades gracefully if Redis becomes unavailable — it should fall back to database sessions and uncached responses without a hard error shown to users
- Verify Redis persistence configuration (AOF or RDB) is enabled to prevent data loss on restart: `redis-cli config get save` and `redis-cli config get appendonly`
- Confirm Redis is not publicly accessible — verify it binds to localhost or private network only: `redis-cli -h 0.0.0.0 ping` should fail from outside the server
- Verify that Redis is protected by a password in production: `redis-cli config get requirepass`
- Test cache hit ratio is healthy (target: > 80%): `redis-cli info stats | grep keyspace_hits` vs. `keyspace_misses`
- Confirm that the queue system (if Redis-backed) and the cache use separate Redis databases or namespaces to avoid key collisions: `redis-cli config get databases`
