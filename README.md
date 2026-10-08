# Security Governance Lab — Equifax Breach Simulation

A hands-on lab that simulates, analyzes, and addresses security governance failures in a controlled Linux/Docker environment, modeled on the conditions behind the 2017 Equifax data breach (CVE-2017-5638, Apache Struts2 S2-045).

This repo contains the full environment setup, vulnerability scanning, attack simulation, governance control implementation, and metrics/reporting pipeline built across four stages, each mapping to a real governance failure pattern from the Equifax case study.

## Overview

The lab walks through a complete governance lifecycle:

1. **Simulate** — stand up a deliberately vulnerable environment (Struts2 + MySQL + no segmentation)
2. **Analyze** — exploit the vulnerability and trace the governance failures behind it
3. **Remediate** — implement formal controls to close the gaps
4. **Report** — build metrics and an executive dashboard to verify the fix holds

The central lesson: Equifax didn't fail because a policy didn't exist, it failed because the policy was never enforced, never measured, and never visible to leadership until it was too late. This lab reproduces that pattern end to end.

## Environment

- Ubuntu Linux (20.04+)
- Docker + Docker Compose
- Python 3
- Required Python packages: `requests`, `matplotlib`

### Containers

| Container | Image | Role |
|---|---|---|
| `web_server` | `vulhub/struts2:2.3.30` | Vulnerable Struts2 app (CVE-2017-5638) |
| `database_server` | `mysql:5.7` | Customer data store, default credentials |
| `monitoring_server` | `ubuntu:20.04` | Placeholder monitoring host |

## Repo Structure

```
security_governance_lab/
├── docker-compose.yml                   # Environment definition
├── database_init/init.sql               # Sample customer data
├── vulnerability_scanner.py             # Scans for Struts, MySQL, and segmentation issues
├── patch_management.py                  # Simulates applying patches, tracks status
├── governance_tracker.py                # Tracks policies, controls, risks, incidents, metrics, audits
├── initialize_governance.py             # Seeds the tracker with sample governance data
├── simulate_attack.py                   # Exploits the Struts vulnerability
├── analyze_governance_failures.py       # Cross-references scan + attack + governance data
├── implement_governance_controls.py     # Applies patches and records formal controls
├── governance_metrics.py                # Generates trend charts, executive dashboard, board report
├── equifax_comparison.md                # Side-by-side comparison with the real breach
├── governance_controls_report.md        # Documents implemented controls
├── governance_metrics_framework.md      # Defines the 4-category metrics model
├── dashboard_user_guide.md              # How to read the executive dashboard
├── Security_Governance_Lab_Report.docx  # Full write-up with screenshots and analysis
└── metrics_visualizations/              # Generated trend chart PNGs
```

## Running the Lab

```bash
# 1. Set up the environment
mkdir -p security_governance_lab && cd security_governance_lab
docker network create secgov_network
docker compose up -d

# 2. Confirm baseline vulnerabilities
./vulnerability_scanner.py

# 3. Simulate an attack and analyze the governance failures behind it
./simulate_attack.py
./analyze_governance_failures.py

# 4. Implement governance controls
./implement_governance_controls.py --all

# 5. Generate metrics, trend charts, and the executive dashboard
./governance_metrics.py
```

Open `executive_dashboard.html` in a browser (or serve it locally with `python3 -m http.server 8000`) to view the live dashboard.

## Key Findings

**Baseline vulnerability scan:**

| Finding | Severity |
|---|---|
| Apache Struts2 S2-045 (CVE-2017-5638) | Critical |
| MySQL default credentials | High |
| Insufficient network segmentation | Medium |

**Governance failures identified:** 8 total, spanning patch management, incident detection, policy implementation, vulnerability management, metrics and measurement, security architecture, and authentication.

**Comparison to Equifax:**

| Governance Failure | Equifax (2017) | This Lab |
|---|---|---|
| Patch Management | Struts vulnerability unpatched for months | Unpatched Struts vulnerability on web server |
| Security Monitoring | Breach undetected for 76 days | No real-time monitoring; attack went unflagged |
| Network Segmentation | Lateral movement across internal network | Web server has direct access to database |
| Authentication | Weak credentials and access controls | Default root credentials on MySQL |
| Policy Implementation | Policies existed but were not enforced | Patch/vulnerability policies existed but unenforced |

**Controls implemented:** automated patch management, network segmentation, database authentication hardening, real-time security monitoring, and executive/board oversight, all tracked as formal governance records with assigned owners.

**Post-remediation metrics:** all five tracked metrics (patch compliance, vulnerability remediation time, security incidents, monitoring coverage, policy compliance) met or exceeded target after controls were applied.

## Full Report

See [`Security_Governance_Lab_Report.docx`](./Security_Governance_Lab_Report.docx) for the complete write-up, including screenshots of each stage, the full Equifax comparison, controls table, metrics table, and executive dashboard.

## Disclaimer

This lab runs in an isolated Docker network for educational purposes only. The vulnerable Struts2 image and simulated attack are not to be exposed to any public network or used against systems you do not own or have explicit permission to test.
