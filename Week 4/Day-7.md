## Ansible Roles

- Implemented a small project using ansible roles
- provisioned a webserver with following 3 roles
    - common `update apt +  install vim, curl, ufw`
    - webserver `install nginx + start it + deploy custom index.html`
    - security `enable ufw + allow ssh and http through firewall`
 ---

# Ansible Roles — Hands-On Project Log
 
A multi-role Ansible project that provisions an EC2 web server from scratch: system prep, nginx deployment, and security hardening — all driven by one master playbook.
 
## What This Project Does
 
```
site.yml
  ├── common      → apt update, install curl/vim/ufw
  ├── webserver   → install nginx, deploy custom index.html, start service
  └── security    → enable UFW, allow SSH (restricted) + HTTP
```
 
Run with:
```bash
ansible-playbook -i inventory site.yml
```
 
---
 
## Key Concepts Learned
 
### 1. Roles vs flat playbooks

Roles split a playbook into reusable, self-contained units (`tasks/`, `handlers/`, `defaults/`, `vars/`, `files/`, `templates/`, `meta/`). Instead of one long playbook, `site.yml` just references roles by name and Ansible runs each role's `tasks/main.yml` in order.
 
### 2. `ansible-galaxy init` vs manual folder creation

Both produce a working role, but `ansible-galaxy init <path>` scaffolds the full standard structure (including `README.md` and `meta/main.yml`) — closer to what real-world/production roles look like, so I switched to using it going forward.
 
### 3. Order matters in `roles:`

`security` runs last intentionally — firewall rules get applied only after nginx is already installed and running, avoiding a self-lockout scenario during testing.
 
### 4. Provisioning, defined

Provisioning = taking a server from a bare/empty state to a fully configured, ready-to-use state. Ansible handles **configuration provisioning** (software, services, settings) on top of infrastructure that already exists — distinct from tools like Terraform, which provision the infrastructure itself (the EC2 instance, networking, etc).
 
---
 
## Bugs I Hit and Fixed
 
| Bug | Cause | Fix |
|---|---|---|
| `apt` module failed with permission denied on `/var/lib/apt/` | Missing `--become` — apt needs root | Added `--become` to escalate privileges |
| `ERROR! this task 'copy' has extra params` | Used YAML syntax (`src: x`) inside an ad-hoc `-a` string | Ad-hoc commands need `key=value`, not `key: value` |
| `Unsupported parameters for (apt) module: Update_cache` | Typo — parameter is case-sensitive | Fixed `Update_cache` → `update_cache` |
| `value of state must be one of: enabled, disabled, reloaded, reset, got: present` | Used `apt`-style `state: present` on the `ufw` module | Changed to `state: enabled` — each module defines its own valid `state` values |
| `ERROR! conflicting action statements: ufw, state` | YAML indentation bug — `state:` was a sibling of `ufw:` instead of nested inside it | Indented `state:` two spaces under `ufw:` |
| Site unreachable after successful playbook run | Two separate causes, stacked | (1) AWS Security Group had no port 80 rule (2) was testing the EC2's **private** IP instead of its **public** IP |
 
---
 
## Security Hardening Applied
 
- SSH (port 22) restricted to a single source IP instead of `0.0.0.0/0`
- Caught and removed a leftover **IPv6** SSH rule (`::/0`) that stayed wide-open even after the IPv4 rule was restricted — both protocol versions need to be locked down separately
- UFW enabled as an OS-level firewall on top of the AWS Security Group
- Confirmed key-based SSH auth only (no password auth)
**Key realization:** firewalls (Security Groups + UFW) are a network-layer control — they decide *which ports are reachable*, not *what happens inside allowed traffic*. They don't protect against SQL injection, XSS, brute-force login attempts, or weak password storage. That's the application layer's job (input validation, hashed passwords, rate limiting, HTTPS, WAF). Useful distinction to carry into any future project with a real login/backend.
 
---
 
## Stack Used
 
- AWS EC2 (Ubuntu)
- Ansible (ad-hoc commands → playbooks → roles)
- nginx
- UFW
- AWS Security Groups
## Next Steps
 
- Add `handlers/main.yml` to the `webserver` role so nginx only restarts when its config actually changes
- Move hardcoded values (package lists, ports) into `defaults/main.yml` as variables
- Push `roles/` to GitHub
 

## Info I like to come revisit

### Why It's Called site.yml
This is just a naming convention, not a requirement. The Ansible community commonly names the top-level playbook site.yml because historically 
it represented `configure my entire site/infrastructure.` You could name it deploy.yml or main.yml and it would work identically — Ansible doesn't care about the filename.

### The Bigger Picture
This is the exact pattern that scales. Right now  we  have 1 server and 3 roles. In a real company, the same site.yml structure might look like:
yaml- hosts: webservers
  roles: [common, webserver, security]

- hosts: databases
  roles: [common, postgres, security]

- hosts: loadbalancers
  roles: [common, nginx_lb, security]
  
Same common and security roles get reused across different server types — that's the entire point of roles. 
This is how real-life production infrastructure is organized.
