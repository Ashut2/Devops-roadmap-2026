## Day 3 — 30 June 2026

- Attempted to SSH into target EC2 to run CPU stress test
- Hit recurring SSH timeout — debugged security group IP mismatch (switched from WiFi to mobile internet)
- After IP update confirmed correct, SSH still failing despite 3/3 status checks, correct security group, public IP assigned, no NACL deny rules
- EC2 Instance Connect and CloudShell SSH both also failed
- Notable: CloudWatch agent still pushing metrics outbound — instance has outbound connectivity but inbound is fully blocked
- Posted issue to Discord community for help
- Stress test and full alarm trigger pending SSH resolution
