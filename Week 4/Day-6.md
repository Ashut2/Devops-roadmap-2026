# Things I revised 
- defined root structure for ansible project of provisioning a production-style grade web server using ansible roles.
- Created a basic structure of ansible roles
```
ansible
|
|
|---Inventory
|---site.yml
|---roles
    |-common\
       |- tasks\
          |-main.yml
    |-webservers
      |---tasks
          |- main.yml 
      |---files
          |- index.html
   |-security
     |-tasks/
        |-main.yml

```
- after this , I gone through documentation of ansible example of jboss server standalone  
running through playbook to get a context on how professional playbooks are written!
