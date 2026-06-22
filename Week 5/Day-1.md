### Day 1 — 23 June 2026

- Attached IAM role with CloudWatchAgentServerPolicy to EC2 instances
- Studied AWS documentation heavily — covered AMI, SSM (Systems Manager), CloudWatch agent prerequisites
- Understood how IAM instance roles give EC2 identity without hardcoded credentials
- Installed and configured CloudWatch agent on target EC2 manually via AWS docs
- Verified CWAgent namespace appearing in CloudWatch console with CPU and network metrics flowing from target-ubuntu

 
 `Next`: Create CloudWatch alarm on cpu_usage_idle
