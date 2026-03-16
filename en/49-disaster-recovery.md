# 49. Disaster Recovery & Rollback

### Backup restoration tested (DB + files)

- Trigger a full database backup manually via the backup script/tool and verify the output file is created in the expected storage location (local disk, S3, remote NAS) within the defined backup window.
- Restore the most recent database backup to an isolated staging environment; verify the restoration completes without errors and the process is fully documented in restoration logs.
- Time the full database restoration from initiation to first query available; record the elapsed time and compare against the defined RTO target.
- After DB restoration, run a row-count comparison query against a known-good snapshot for at minimum: `orders`, `customers`, `products`, `order_items`, `sessions` tables. Confirm no table differs by more than 0 rows.
- Restore the most recent file backup (media/uploads, configuration files, application code) to the staging environment; verify directory tree matches expected structure with `diff -rq` or equivalent.
- Time the full file restoration from initiation to last file written; record against RTO target.
- Trigger a backup during peak activity (active orders, file uploads in progress) and verify the resulting backup is consistent (no partial writes, no corrupt images, no truncated records).
- Simulate a mid-backup network failure to off-site storage; verify the backup process either retries successfully or logs a clear failure alert without silently producing an incomplete archive.
- Restore a backup that is 7 days old; verify the shop is functional and that all orders placed in the last 7 days are correctly absent (confirming point-in-time accuracy rather than stale data masquerading as current).
- Verify that backups are encrypted at rest; confirm the backup file cannot be read without the decryption key.
- Confirm backup files are stored in at least two geographically separate locations (e.g., primary server + off-site S3 bucket).
- After full restoration, place a test order end-to-end (add to cart, checkout, payment sandbox) and verify the order is written to the restored database correctly.

---

### Backup integrity check (no empty/corrupt backups)

- After each scheduled backup run, verify the backup file size is above a defined minimum threshold (e.g., DB dump must be >1 MB; file archive must be >50 MB); alert if the threshold is not met.
- Attempt to open/decompress the most recent backup archive; confirm it extracts without errors (exit code 0, no CRC errors, no truncation warnings).
- Validate the DB dump file contains expected SQL markers: presence of `CREATE TABLE`, `INSERT INTO`, and `COMMIT` statements; alert if any are missing.
- Run a scheduled automated integrity check (e.g., `mysqlcheck`, `pg_restore --list`, or equivalent) against each new backup immediately after creation; log and alert on any failure.
- Compare the checksum (SHA-256) of the backup file stored at creation time against the checksum of the file retrieved from storage the following day; confirm they match (detects storage corruption or tampering).
- Restore the backup to a throwaway schema and run `SELECT COUNT(*)` on the 10 largest tables; compare against the source counts recorded at backup time.
- Simulate a backup that writes 0 bytes (e.g., a disk-full condition); verify the monitoring system raises an alert within 15 minutes and does NOT mark the backup as successful.
- Verify that backup logs are retained for at least 90 days and include: start time, end time, file size, storage path, checksum, and success/failure status.
- Attempt to restore from a backup where the archive has been deliberately corrupted (flip one byte); confirm the restoration process detects and reports the corruption rather than silently loading bad data.
- Check that the backup rotation policy is enforced: confirm that backups older than the defined retention period (e.g., 30 days) are automatically purged and that at least the last N backups are always retained.

---

### Rollback plan for failed deployment

- Deploy a known-bad release to staging (e.g., a build with a syntax error or broken migration); execute the documented rollback procedure and measure the time from decision-to-rollback to site fully restored on the previous version.
- Verify that the rollback procedure is written in a runbook with step-by-step commands; confirm a developer unfamiliar with the system can execute it successfully using only the runbook.
- After rollback, confirm the application version identifier (e.g., `APP_VERSION` env var, git commit hash in footer, or `/health` endpoint) reflects the previous release, not the failed one.
- Execute a rollback when the database migration has already run (schema change deployed); verify the rollback either reverses the migration cleanly or the runbook documents which manual steps are required.
- Simulate a rollback while live traffic is active; confirm zero orders are lost (check order IDs before and after rollback), no duplicate orders are created, and no payment intents are left in an ambiguous state.
- Verify the CI/CD pipeline has a one-click or single-command rollback mechanism; test that it completes without requiring manual server access.
- After rollback, run the full smoke test suite (homepage loads, product page loads, add to cart, checkout step 1 reachable) and confirm all pass.
- Confirm that static assets (CSS, JS, images) served after rollback match the rolled-back code version, not the failed deployment (no version mismatch between HTML and assets).
- Test rollback on a deployment that involved a config file change (e.g., `.env` or `config/app.php`); verify the old config is restored alongside the old code.
- Verify that post-rollback monitoring dashboards (error rate, response time, 5xx rate) return to pre-deployment baseline within 5 minutes and no alerts remain firing.

---

### RPO/RTO defined and tested

- Confirm that RPO (Recovery Point Objective) and RTO (Recovery Time Objective) values are formally documented in the disaster recovery plan; verify they are reviewed and approved by a stakeholder.
- Run a full DR drill: simulate a total server loss at a random time; measure the actual time from incident declaration to first customer-facing page loading; compare against the documented RTO.
- Determine the maximum data loss in the drill by checking the timestamp of the most recent backup successfully restored; calculate the gap between that timestamp and the simulated failure time; compare against the documented RPO.
- If RPO is defined as, e.g., 1 hour: verify that backups are scheduled with a frequency of at most 1 hour and that the most recent backup is always within that window when checked at any random time.
- If RTO is defined as, e.g., 4 hours: verify that the full restoration procedure (DB + files + DNS propagation + smoke test) can be completed within that window; run it against a clean environment and time it.
- After the DR drill, document actual RPO achieved and actual RTO achieved; confirm both meet the defined targets, or escalate and revise targets/procedures if they do not.
- Test partial recovery scenario: only the database is lost (files intact); measure the time to restore DB only and confirm it is significantly below the full RTO.
- Verify that all team members responsible for DR have completed at least one hands-on restoration drill within the last 6 months; check training records.
- Confirm that contact lists, escalation paths, and credentials needed for DR are stored separately from the primary system (e.g., in a password manager or printed runbook) so they remain accessible when the system is down.
- After the full DR drill, conduct a post-mortem: identify any steps that took longer than expected, any commands that failed, or any missing documentation; update the runbook accordingly.

---

### Failover on server outage

- Simulate a primary web server crash (stop the service or shut down the VM); verify that traffic is automatically redirected to a standby/failover server or load balancer target within the defined failover time.
- Measure the time from primary server going offline to the first successful HTTP 200 response from the failover target; confirm it meets the documented SLA.
- During failover, verify that the customer session is preserved or gracefully handled: a user mid-checkout should either complete the checkout on the failover node or see a clear, user-friendly error rather than a silent hang.
- Confirm that the failover server has access to the same shared database or a replicated copy; verify no order data written in the 60 seconds before failover is lost.
- Simulate a failover during an active payment processing flow; verify the payment provider webhook retries are handled correctly and no double charges occur.
- After failover is active, run the full smoke test suite against the failover endpoint; confirm all critical paths (homepage, product, cart, checkout) are operational.
- Test failover of the database tier separately: stop the primary DB server and verify the application connects to the replica or failover DB within the defined timeout; confirm read/write operations succeed.
- Verify that DNS TTLs and health check intervals are configured to enable failover within the documented RTO; confirm current DNS configuration reflects this.
- Simulate a partial outage (one node in a multi-node cluster); verify the load balancer removes the unhealthy node from rotation within the health check interval and the remaining nodes handle traffic without errors.
- After the failover period, simulate primary server recovery and verify traffic is restored to the primary without service interruption; confirm no duplicate job processing or split-brain data issues occur on failback.
- Confirm that alerting fires within 2 minutes of the primary server going offline; verify the on-call engineer receives the notification (PagerDuty, SMS, email) and the alert contains enough context to act.

---

### Database replication working

- Verify the replication lag between primary and replica(s) is below the defined threshold (e.g., <5 seconds) under normal load; check the replication status dashboard or run `SHOW REPLICA STATUS`.
- Write a test record to the primary database and verify it appears on the replica within the defined replication lag SLA; measure the actual propagation time.
- Simulate a high-write load (bulk import of 10,000 product records or 1,000 orders); measure replication lag during and after the load; confirm it recovers to below threshold within a defined window.
- Simulate primary DB failure and promote the replica to primary; verify the promotion completes without data loss, and the application reconnects automatically or with minimal manual intervention.
- After replica promotion, verify the application can perform writes to the new primary; run an end-to-end order placement test.
- Verify that the replica is excluded from accepting writes under normal operation; confirm any attempted write to the replica returns an appropriate error and does not silently succeed.
- Check that binary log retention on the primary is configured to cover at least the RPO window; confirm logs are not purged before replication has consumed them.
- Simulate a replication break (disconnect replica for 10 minutes, then reconnect); verify replication resumes automatically and catches up without manual intervention; confirm no data gaps.
- Verify monitoring raises an alert if replication lag exceeds the defined threshold; test by pausing the replica's SQL thread and confirming an alert fires within the configured check interval.
- Confirm that backups are taken from the replica (not the primary) where applicable; verify this does not introduce stale data and does not interrupt replication.

---

### Automatic alerts on system outage

- Bring down the web server process; confirm an alert is triggered and delivered to the on-call channel (Slack, PagerDuty, email) within 2 minutes; verify alert content includes: service name, affected host, time of detection, severity.
- Stop the database service; confirm a separate DB-down alert fires within the configured check interval; verify it is routed to the correct escalation path.
- Saturate CPU to >95% for 5 minutes; confirm a CPU alert fires and resolves automatically when load drops below threshold.
- Fill disk to >90% on the application server; confirm a disk space alert fires and includes the mount point and current usage percentage.
- Simulate a failed SSL certificate (expired or mismatched); confirm an SSL expiry alert fires at least 30 days before expiry and again at 14 days; verify the alert recipient list is correct.
- Stop the cron job responsible for sending order confirmation emails; confirm a "job not running" alert fires within the defined check window (e.g., if job should run every 5 minutes, alert after 10 minutes of absence).
- Verify that all alerts have a defined owner and escalation path documented; simulate a primary contact being unavailable and confirm the escalation to the secondary contact triggers after the defined acknowledgment timeout.
- Test alert deduplication: bring a service down and back up rapidly; confirm alerts are not spammed (one alert per incident, one resolution notification).
- Simulate a monitoring system failure itself (stop the monitoring agent); confirm a "dead man's switch" or watchdog alert fires to notify that monitoring is offline.
- Review alert fatigue: check the alert history for the past 30 days; confirm there are no recurring false-positive alerts that could cause the team to ignore real incidents; tune thresholds as needed.
- After any alert fires and is resolved, verify that a resolution notification is sent automatically so the team knows the incident is over without manually checking the dashboard.
