# DevOps & Production Infrastructure: Set 1
## Linux Fundamentals, Shell Automation & Environment Parity

## 📘 10 Conceptual Questions
1. **Linux Filesystem Hierarchy & Permissions:** How do `/etc`, `/var`, `/usr`, and `/opt` differ in purpose? Why should application data never live in `/root` or `/home`, and how do `rwx` permissions + ownership prevent privilege escalation?
2. **Process Lifecycle & Signal Handling:** What happens when you run `systemctl stop` or `kill -15`? Explain the difference between `SIGTERM` (graceful) and `SIGKILL` (forced), and why apps must handle shutdown signals to avoid data corruption or stuck connections.
3. **Shell Scripting Safety:** Why is `#!/bin/bash` alone dangerous in production? Explain `set -euo pipefail`, proper quoting, exit codes, and why unhandled errors in scripts cause silent failures or partial deployments.
4. **Environment Parity Philosophy:** What causes "works on my machine" syndrome? How do configuration drift, missing dependencies, and hardcoded paths break parity, and why is treating infrastructure as reproducible code the only sustainable fix?
5. **Configuration vs. Secrets:** Why should `.env` files never be committed? How do environment variables, secret managers, and config injection differ in security, and what happens when secrets leak into version control history?
6. **Networking Basics for Web Apps:** How do ports, firewalls, and reverse proxies interact? Why should Laravel bind to `127.0.0.1` or a private interface instead of `0.0.0.0`, and how does DNS resolution affect external API calls and caching?
7. **Log Management & Rotation:** Why should containerized or systemd-managed apps log to `stdout`/`stderr` instead of rotating files manually? How do `logrotate`, journalctl, and centralized log collectors prevent disk exhaustion and enable debugging?
8. **Package Management & Version Pinning:** Why is `apt upgrade` or `npm install` without lockfiles dangerous in production? Explain dependency drift, reproducible builds, and how to pin versions without sacrificing security patches.
9. **Principle of Least Privilege:** Why is running web servers, queue workers, or cron jobs as `root` a critical security flaw? How do dedicated service users, `sudo` restrictions, and capability bounding reduce blast radius during compromises?
10. **Automation vs. Manual Ops:** What breaks when deployments rely on SSH, manual config edits, or "copy-paste" steps? How do scripts, version-controlled configs, and idempotent commands transform fragile operations into reliable pipelines?

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Secure Linux User Setup**<br>Create a non-root user, configure SSH key auth, disable root/password login, set up `sudo` with limited commands. Verify login works and root access is blocked. | SSH keys required. Root login disabled. `sudo` works for allowed commands only. `whoami` returns app user, not root. |
| 2 | **Production-Ready Bash Script**<br>Write `setup-server.sh` that installs dependencies, creates directories, sets permissions, and configures basic services. Use `set -euo pipefail`, trap errors, and exit with proper codes. | Script runs idempotently. Fails cleanly on missing packages. Logs actions. Exit code 0 on success, >0 on failure. No partial state left behind. |
| 3 | **Environment Parity Audit**<br>Compare dev, staging, and prod configs. Document differences in PHP versions, cache drivers, queue workers, and log paths. Create a parity checklist and fix 3 drift points. | Checklist covers 5+ config areas. Dev/staging/prod aligned on critical settings. Drift documented and resolved. Parity verification command/script created. |
| 4 | **Log Rotation & Console Output**<br>Configure `logrotate` for app logs. Modify Laravel to log to `stdout`/`stderr` in production. Verify logs rotate at 10MB, retain 5 archives, and disk usage stays stable. | `logrotate` runs without errors. Laravel logs to console in prod. Disk usage doesn't grow indefinitely. Logs queryable via `journalctl` or tail. |
| 5 | **Systemd Service for Queue Worker**<br>Create `laravel-worker.service` with `Restart=always`, `User=www-data`, and proper environment loading. Enable, start, and simulate failure (kill PID). Verify auto-recovery. | Service starts on boot. Auto-restarts on crash. Logs visible in `journalctl`. No manual intervention needed for recovery. |
| 6 | **Firewall Hardening with UFW**<br>Configure `ufw` to allow only 22 (SSH), 80 (HTTP), 443 (HTTPS). Block all other inbound. Test with `nmap` or `telnet`. Document allowed ports and reasoning. | Only 3 ports open. SSH rate-limited. `nmap` shows filtered/closed for others. Rules survive reboot. Changes documented. |
| 7 | **Secret Extraction & Safe Injection**<br>Find hardcoded credentials in code/config. Move to environment variables or a secrets manager. Verify no secrets in `git log` or `.env.example`. Test app startup with injected vars. | Zero secrets in repo. `.env.example` contains placeholders only. App starts with external vars. Secret rotation documented. |
| 8 | **Docker Local Parity Baseline**<br>Create `docker-compose.yml` matching prod services (app, DB, Redis, Mailpit). Verify `docker compose up` starts identical stack. Document differences from prod. | One-command local setup. Services mirror production architecture. No manual dependency installs. Parity gaps documented. |
| 9 | **Shell Error Handling & Cleanup**<br>Add `trap` to a deployment script that cleans up temp files, releases locks, and logs failures on `EXIT`/`ERR`. Simulate mid-script failure. Verify cleanup runs. | Temp files removed on failure. Locks released. Error logged with context. Script never leaves system in broken state. |
| 10| **Operations Runbook Creation**<br>Write `DEPLOYMENT.md` and `TROUBLESHOOTING.md` covering: setup steps, log locations, restart commands, common errors, and parity checks. Verify a new dev can follow it without help. | Docs cover 5+ critical workflows. Commands copy-paste ready. Troubleshooting flowcharts/steps included. Tested by fresh setup. |
