# Lab Title: Secure Root SSH Access

## Scenario
Simulated securing root SSH access on a production Linux server to prevent unauthorized login.

## Objective
Disable direct root login and enforce secure access policies.

## Commands Used
- ssh usr-name@ip_address
- useradd
- passwd
- vi /etc/ssh/sshd_config
- sudo sshd -t : empty o/p shows config is donne succesfully
- systemctl restart sshd

## Steps Performed
1. Created a non-root admin user.
2. Modified sshd_config to disable PermitRootLogin.
3. Restarted SSH service.
4. Verified access behavior.

## Verification
- Attempted root SSH login → Access denied.
- Logged in using new admin user → Successful.

## Real-World Use Case
In production environments, direct root SSH login is disabled to reduce attack surface and enforce auditability.

## Key Learning
- SSH hardening basics
- Service restart impact
- Importance of controlled privilege escalation
