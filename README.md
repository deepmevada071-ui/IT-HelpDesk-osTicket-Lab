# IT Help Desk Simulation — osTicket

> Self-hosted osTicket help desk on XAMPP. Configured departments, help topics, SLA tiers, and resolved a realistic queue of Tier-1 tickets end-to-end.

## Overview

Every IT support job runs on a ticketing system — ServiceNow, Jira, Zendesk, FreshService, osTicket. This lab installs the open-source one (osTicket) on a local LAMP stack (XAMPP) and uses it exactly the way a help desk team would on day one: triage, assign, comment, resolve, close, SLA-track.

## Objectives

- Stand up a fully working osTicket instance on a local XAMPP stack
- Configure departments, help topics, agents, and SLA tiers that mirror a real small company
- Operate the system as both client (user submitting tickets) and agent (technician resolving them)
- Resolve a queue of 6+ tickets covering the most common Tier-1 issues
- Document resolution steps in agent notes so each ticket reads like a real audit trail

## Tools & Technologies

| Tool | Purpose |
|---|---|
| XAMPP (Apache, MySQL, PHP) | Local web server stack |
| osTicket (latest stable) | Help desk application |
| phpMyAdmin | Database management UI |
| Windows 10/11 | Host OS |

## Lab Architecture

```
   ┌───────────────────────────────────────────┐
   │  Host PC (Windows)                        │
   │                                           │
   │   Browser (Chrome)                        │
   │      │                                    │
   │      ├── http://localhost/osticket/       │
   │      │       Client Portal (user view)    │
   │      │                                    │
   │      └── http://localhost/osticket/scp    │
   │              Staff Control Panel (agent)  │
   │                                           │
   │   Apache  ──►  PHP  ──►  osTicket         │
   │                          │                │
   │                          ▼                │
   │                      MySQL (osticket_db)  │
   └───────────────────────────────────────────┘
```

## Step-by-Step Setup

### Phase 1 — Install XAMPP

1. Download from apachefriends.org → install to `C:\xampp`
2. Open XAMPP Control Panel → **Start** Apache and MySQL
3. Visit `http://localhost` — XAMPP welcome page confirms it's up
   - If Apache won't start: edit `httpd.conf`, change `Listen 80` → `Listen 8080`

### Phase 2 — Create the osTicket Database

1. `http://localhost/phpmyadmin` → **Databases** tab
2. Create database: `osticket_db` (default collation)
3. Note credentials: user `root`, password blank, host `localhost`

### Phase 3 — Drop osTicket into XAMPP

1. Download osTicket Core from osticket.com/download
2. Extract → rename `upload/` to `osticket/`
3. Copy `osticket/` into `C:\xampp\htdocs\`
4. In `C:\xampp\htdocs\osticket\include\` → rename `ost-sampleconfig.php` → `ost-config.php`
5. Right-click `ost-config.php` → Properties → uncheck **Read-only**

### Phase 4 — Run the Web Installer

1. Visit `http://localhost/osticket/setup`
2. Resolve any red X on PHP extensions (uncomment in `php.ini`, restart Apache)
3. Fill form:
   - Helpdesk Name: `Deep's IT Help Desk`
   - Admin: your name / `admin@mylab.com` / `admin` / strong password
   - DB: `localhost` / `osticket_db` / `root` / *(blank)*
4. Install Now → success
5. **Post-install**: delete `C:\xampp\htdocs\osticket\setup\`, re-enable read-only on `ost-config.php`

### Phase 5 — Configure the Help Desk

**Departments** (Admin Panel → Agents → Departments):
- IT Support
- Networking
- Hardware

**Help Topics** (Admin Panel → Manage → Help Topics):
- Password Reset
- Software Installation
- Printer Issue
- Network Connectivity
- Hardware Problem
- New Employee Setup

**SLA Plans** (Admin Panel → Manage → SLA):

| SLA | Grace Period | Schedule | Use For |
|---|---|---|---|
| SEV-A | 1 hour | 24/7 | Critical (server down, mass outage) |
| SEV-B | 4 hours | 24/7 | High (department affected) |
| SEV-C | 8 hours | Business hours | Normal (single user) |

## Simulated Tickets Resolved

| # | Topic | Subject | Priority | SLA | Resolution |
|---|---|---|---|---|---|
| 001 | Password Reset | "Can't log in to my email" | Normal | SEV-C | Reset password in AD, forced change at next login |
| 002 | Network Connectivity | "Wi-Fi keeps dropping in conference room" | High | SEV-B | Verified AP channel, restarted controller, asked facilities to relocate AP |
| 003 | Printer Issue | "Print jobs stuck in queue" | Normal | SEV-C | Cleared print spooler, reinstalled driver, tested print |
| 004 | Software Installation | "Need Visual Studio installed on new laptop" | Normal | SEV-C | Verified license, ran silent installer, validated activation |
| 005 | Hardware Problem | "Laptop won't power on" | High | SEV-B | Power-cycled, checked battery health, escalated to RMA |
| 006 | New Employee Setup | "Onboarding for new hire Monday" | Normal | SEV-C | Created AD account, assigned licenses, prepped laptop image, scheduled orientation |

Each ticket includes:
- Initial user message
- 1–3 internal/agent notes showing diagnostic steps
- Final resolution comment
- Closed with the matching help topic + SLA

## Screenshots

> Add to `/screenshots/` as you complete:

- `01-xampp-running.png` — Apache and MySQL green
- `02-osticket-installer.png` — Setup completion
- `03-staff-dashboard.png` — Agent SCP with open tickets
- `04-help-topics.png` — Configured help topic list
- `05-sla-plans.png` — SEV-A/B/C SLA configuration
- `06-ticket-thread.png` — A full resolved ticket showing notes + resolution
- `07-departments.png` — Departments page
- `08-client-portal.png` — User-facing ticket submission form

## What I Learned

- The mechanical difference between **client portal** and **staff control panel** views
- Why SLA tiers exist and how they drive prioritization at scale
- How help topics route tickets to the correct department automatically
- The value of agent internal notes vs. user-visible replies for audit trails
- Common PHP extension dependencies (`imap`, `gd`, `mbstring`) and how to enable them in `php.ini`
- The post-install hardening steps that real shops do (delete `/setup`, re-lock `ost-config.php`)

## Resume Bullet

> Deployed osTicket on a local XAMPP stack; configured departments, help topics, SLA tiers, and agent roles; processed and resolved a queue of 6 simulated Tier-1 tickets covering password resets, network issues, hardware faults, and new-hire onboarding.

## Related Projects

- [Active-Directory-Home-Lab](https://github.com/deepmevada071-ui/Active-Directory-Home-Lab) — supplies the AD environment for password-reset / onboarding tickets
- [Network-Troubleshooting-Lab](https://github.com/deepmevada071-ui/Network-Troubleshooting-Lab) — the network behind the connectivity tickets

---

**Deep Mevada** · Computer Science · University of Lethbridge · [deepaboutme.site](https://deepaboutme.site) · [LinkedIn](https://www.linkedin.com/in/deep-mevada-242327260/)
