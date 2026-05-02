`this is the continuetion of 1st config management with ansible`
## What did I learn 
- Inventory file contains the IP Address of target servers.
- ansible adhoc commands are there to execute 1 or two task at a time atmost
- whereas ansible playbook is a file containing multiple ansible commands which has to be execute in order. 
-  adhoc command eg: `ansible -i inventory all -m "shell" -a "touch devopsclass"` 
This command will create a file named `devopsclass` , you can go to target server to verify with `ls -ltr` whether file 
is there or not. 

## What did I execute 
- Deployed the portfolio footer.tsx changes.
- created a specific public readme repo for repo-visitors so that they can see how my portfolio is deployed.
- now again added commits here in this note-taking-log-repo.
- Settuped the passwordless Auth for Ansible and target server using `ssh-keygen`
