## Ansible Roles

- Implemented a small project using ansible roles
- provisioned a webserver with following 3 roles
    - common `update apt +  install vim, curl, ufw`
    - webserver `install nginx + start it + deploy custom index.html`
    - security `enable ufw + allow ssh and http through firewall`
 ---
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
