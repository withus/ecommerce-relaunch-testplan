# 52. Load & Stress Tests (detailed)

### Spike Tests: sudden traffic increase (e.g., newsletter send)

- **Baseline → spike simulation with k6:** Run a k6 script that holds 10 VUs for 2 minutes (baseline), then ramps to 500 VUs in 30 seconds, holds for 5 minutes, then ramps back down. Success criteria: p95 response time stays below 3 s during spike; error rate below 1%.
- **Newsletter-blast scenario with Artillery:** Model a realistic distribution — homepage (40%), category pages (35%), product pages (25%) — matching the click pattern of a typical newsletter campaign. Fire 2,000 requests/minute for 3 minutes. Success criteria: no 5xx responses; cart add-to-cart endpoint p99 below 5 s.
- **Homepage endurance after spike with Locust:** After the spike subsides, keep 100 concurrent users active for 15 minutes to verify the system stabilises (no gradual memory leak / connection pool exhaustion visible in response-time trends).
- **Cold-cache spike test:** Flush all Redis/Varnish/page caches, then immediately ramp to 300 VUs with k6. Validates that origin servers handle cache-miss storm. Success criteria: TTFB below 800 ms for homepage even without cache.
- **Database connection pool saturation test:** Instrument the DB connection pool (e.g., via New Relic or Datadog). During a 500-VU spike, confirm max active connections never exceeds the configured pool limit and that requests queue rather than return errors. Alert threshold: pool utilisation above 90% for more than 60 seconds.
- **CDN cache-hit ratio during spike:** Use a k6 script with randomised query strings versus clean URLs to verify CDN cache-hit ratio remains above 85% for static assets during the spike. Measure via CDN analytics dashboard.
- **Auto-recovery validation:** After spike ends, confirm p95 response time returns to baseline (below 500 ms for homepage) within 60 seconds. Automated k6 check on the post-spike steady-state window.
- **Third-party widget timeout behaviour under spike:** During a 300-VU spike, throttle the external payment widget and review platform endpoints (via a proxy/WireMock) to 5-second latency. Verify the shop renders normally and shows graceful fallback messages — no full-page errors.

---

### Soak Tests: sustained load over hours

- **8-hour baseline soak with k6:** Maintain 50 concurrent users (representing a realistic daily-traffic average) for 8 hours. Monitor p95 response time trend — it must not grow by more than 20% between hour 1 and hour 8 (indicates memory leaks or connection-pool shrinkage).
- **Memory leak detection during soak:** Collect PHP-FPM memory usage (via `pm.status` endpoint or Prometheus exporter) every 5 minutes during the soak. Alert if average worker memory grows by more than 10 MB/hour.
- **Session storage saturation test:** During a 4-hour soak with 200 authenticated users, monitor Redis keyspace size and TTL distribution. Confirm expired sessions are evicted correctly and keyspace does not grow unboundedly.
- **Log file and disk I/O soak test:** Run a 4-hour soak and monitor disk write throughput for application logs, access logs, and error logs. Confirm disk usage does not approach 80% of partition capacity. Alert if error log grows faster than 1 MB/hour.
- **Database slow-query accumulation during soak:** Enable slow-query logging (threshold 500 ms) during the 8-hour soak. Acceptable outcome: zero new slow-query patterns introduced; existing known slow queries are within previously established baselines.
- **Cron-job interference test:** Run the soak while scheduled tasks (price import, inventory sync, sitemap generation) execute at their normal intervals. Response times during cron execution windows must not degrade more than 50% compared to windows without cron activity.
- **Checkout conversion rate stability during soak:** Simulate a realistic funnel (homepage → category → PDP → cart → checkout → order) with Locust for 2 hours at 30 concurrent users. Track simulated conversion rate: it must remain within 2 percentage points of the pre-soak baseline, indicating no session or state corruption over time.
- **Connection pool recycle test:** Restart PHP-FPM workers (graceful restart via `kill -USR2`) mid-soak with 50 VUs active. Verify zero failed requests during the restart window — requests queue and are served by new workers.

---

### Checkout under load (parallel orders)

- **Concurrent order placement with k6:** Simulate 100 users simultaneously clicking "Place Order" within a 5-second window. Use k6 `group()` blocks to track each checkout step separately. Success criteria: all 100 orders created exactly once; no duplicate order records in DB; p95 checkout completion below 8 s.
- **Race-condition test on last-item stock:** Pre-set product stock to 1. Script 20 concurrent users with k6 to add the same product to cart and complete checkout simultaneously. Exactly 1 order must succeed; all others must receive a clear "out of stock" error. Verify DB stock column equals 0 post-test.
- **Payment gateway throughput test:** Run 50 concurrent checkout completions through Stripe test mode using k6. Measure PaymentIntent creation latency. Success criteria: p99 below 4 s; zero double-charge incidents (verify via Stripe dashboard event log).
- **Checkout session isolation under parallel load:** Use Artillery to run 200 concurrent checkout sessions with different user accounts. After completion, verify each order is associated with the correct user and address — no session bleed between users.
- **Order confirmation email queue under burst:** After 200 simultaneous order completions, verify all 200 confirmation emails are dispatched within 5 minutes. Monitor queue depth (Redis/SQS) — it must drain completely within the SLA window.
- **Inventory lock contention test (JMeter):** Use JMeter thread groups to simulate 50 users adding the same variant to cart and 50 users completing purchase of it simultaneously. Measure database lock wait times. No deadlock errors allowed; lock wait time p99 below 500 ms.
- **Browser-level checkout load test with Playwright + k6 browser:** Run 20 browser-level sessions (not just HTTP) through the full checkout flow. Validates that JavaScript-rendered payment widgets (Stripe Elements, PayPal SDK) load and function correctly under parallel load.
- **Partial payment failure recovery under load:** Simulate 10% of concurrent checkout requests receiving a payment gateway timeout (via WireMock fault injection). Verify: failed users receive a clear error; their cart is preserved; no money is debited; successful users are unaffected.

---

### Database performance with large catalogs (>10,000 products)

- **Full-catalog search query benchmark:** Load a DB fixture with 10,000 products, 500 categories, and 5 million product-attribute rows. Run k6 against the search endpoint at 100 VUs. Success criteria: p95 search response below 500 ms; EXPLAIN plan shows index usage (no full-table scans).
- **Category page pagination performance:** With 10,000+ products, benchmark category page loads for pages 1, 50, 100, and 500 via k6 (20 VUs each). Deep pagination (page 500) p95 must remain below 1 s. Flag any query using OFFSET without cursor-based pagination.
- **Filter combination stress test with JMeter:** Configure JMeter to request all 2^5 = 32 combinations of 5 active filters concurrently at 50 VUs. Monitor query execution time in slow-query log. No combination may exceed 2 s p95.
- **Product import vs. live traffic concurrency:** Trigger a bulk product import (5,000 records via CSV) while simulating 100 concurrent browsing users with k6. Category and search response times must not degrade more than 30% during the import window.
- **Autocomplete / Elasticsearch query throughput:** Fire 500 requests/second of 3-character search prefix queries against the autocomplete endpoint. Success criteria: p99 below 200 ms; zero 503 errors. Tool: Artillery with custom processor for random prefix generation.
- **Database index regression test post-migration:** After the relaunch data migration, run `ANALYZE TABLE` on all product, category, and order tables. Execute a pre-defined set of 20 representative queries from production slow-query history and compare EXPLAIN output against pre-migration baseline. Any new full-table scan is a blocker.
- **ORM N+1 detection under catalog load:** Use Clockwork or Laravel Telescope query log during a k6 run across 50 product pages. Flag any page that executes more than 20 DB queries. Target: maximum 10 queries per product detail page.
- **Read replica routing under catalog load:** Verify that catalog read queries (search, listing, PDP) are routed to read replicas by monitoring per-host query counts in the DB monitoring dashboard during a 200-VU k6 run. Write queries (order creation, cart update) must go to the primary only.

---

### CDN failover on CDN outage

- **CDN origin-fallback drill:** Disable the CDN (or block CDN IPs via firewall rule) while k6 runs a 50-VU load against the shop. Verify traffic automatically routes to the origin server. Success criteria: no user-visible 5xx; response time increase stays below 3× CDN-served baseline.
- **CDN failover latency measurement:** Using k6 with a custom scenario, measure the elapsed time from CDN disablement to first successful origin-fallback response. Target: automatic failover within 30 seconds.
- **Static asset availability without CDN:** During the CDN-outage drill, verify that product images, CSS, and JS files are served directly from the origin or a secondary CDN edge. Check for mixed-content errors using the browser (Chrome DevTools or Playwright network inspector).
- **Cache-control header validation for origin fallback:** Verify that origin servers return correct `Cache-Control` and `ETag` headers during failover, ensuring browsers can still cache assets locally. Tool: k6 with response header assertions.
- **CDN reconnection and cache warming test:** After re-enabling the CDN, measure how long it takes for cache-hit ratio to recover to baseline (above 85%). Script automated k6 warm-up traffic across the top 200 URLs (sourced from analytics) to simulate real user patterns.
- **Geographic failover test (multi-region CDN):** If multiple CDN PoPs are in use, disable one regional PoP and verify traffic shifts to the next nearest PoP with degradation of less than 100 ms added latency for the affected region.
- **CDN outage alerting test:** Confirm that the monitoring system (e.g., Datadog, Pingdom) fires an alert within 2 minutes of CDN availability dropping below 99%. Alert must reach on-call engineer via PagerDuty/Opsgenie.

---

### Auto-scaling tested (if cloud hosting)

- **Scale-out trigger verification with k6:** Gradually ramp from 10 to 1,000 VUs over 10 minutes. Monitor cloud provider metrics (AWS EC2 Auto Scaling, GCP MIG, DigitalOcean Droplet autoscaling) and confirm new instances are provisioned when CPU exceeds the 70% threshold. New instances must join the load balancer within 3 minutes.
- **Scale-out latency measurement:** Track the time delta between first breach of CPU threshold and first request served by the new instance. Target: new capacity available within 90 seconds (for containerised / pre-warmed AMI setups).
- **Scale-in after load drops:** After the 1,000-VU ramp-down, verify excess instances are terminated within 15 minutes without prematurely killing instances that are still handling active requests (connection draining / graceful shutdown).
- **Sticky-session behaviour during scale-out:** If session affinity is enabled (e.g., sticky cookies on the load balancer), confirm that existing user sessions are not disrupted when new instances come online. Tool: k6 with session cookies preserved across requests.
- **Blue/green or rolling deployment under load:** During a rolling deployment (2 instances updating at a time), run a 200-VU k6 test. Zero 5xx errors allowed; p95 response time must not spike above 2× normal baseline during the deployment window.
- **Database connection pool scaling:** During a 10-instance scale-out event, verify that total DB connections do not exceed the RDS/MySQL `max_connections` limit (use PgBouncer / ProxySQL if needed). Monitor via DB dashboard.
- **Cost-scaling efficiency check:** After the load test, review the cloud bill estimate. At 1,000 VUs, the number of provisioned instances must not exceed a pre-agreed cap (e.g., 8 app servers), ensuring the autoscaling policy does not over-provision.
- **Autoscale event logging and alerting:** Every scale-out and scale-in event must produce a CloudWatch/Stackdriver log entry and trigger a non-critical notification (Slack) so the team is aware of autoscaling activity.

---

### Recovery time after crash measurement

- **Hard crash simulation with process kill:** Kill all PHP-FPM / application server processes via `kill -9` while 50 VUs are active (k6). Measure time from crash to first successful response after the process manager (systemd/supervisord/Kubernetes) restarts them. Target RTO: below 30 seconds.
- **Database primary failover RTO test:** Trigger a manual primary DB failover (e.g., AWS RDS Multi-AZ failover, or `systemctl stop mysql` on primary). Measure time from failover trigger to shop returning 200 responses. Target RTO: below 60 seconds. Record exact timestamps in test log.
- **Full server reboot recovery test:** Reboot the application server (staging environment) and measure total downtime: from reboot initiation to first successful health-check response from the load balancer. All services (PHP-FPM, Redis, queue workers) must auto-start. Target: below 90 seconds total.
- **Cache layer crash and cold restart:** Stop Redis entirely. Verify the application degrades gracefully (serves requests from DB, no fatal errors). Restart Redis and confirm it repopulates without manual intervention. Measure time to return to normal cache-hit ratios.
- **Queue worker crash and message durability:** Kill all queue worker processes. Verify that queued jobs (emails, webhook deliveries) are durably stored (not lost). Restart workers and confirm all pending jobs are processed within their SLA. Tool: custom script to count queue depth before/after.
- **RPO validation after DB crash:** Simulate a crash followed by DB restore from the most recent automated backup. Verify that no orders placed in the last backup interval (typically 5–15 minutes) are lost or corrupted. Compare order count in restored DB against a reference export.
- **Automated health-check and self-healing test:** Verify the load balancer health check (e.g., `GET /health`) correctly detects a crashed instance and removes it from rotation within 2 health-check intervals (e.g., 2 × 10 s = 20 s). Confirm no traffic is sent to the unhealthy instance during recovery.
- **Post-crash transaction integrity check:** After recovery from a simulated crash mid-checkout, verify that partial transactions (e.g., PaymentIntent created but order not written) are handled: either rolled back cleanly or completed on next user action. No orphaned payment records without matching orders.
