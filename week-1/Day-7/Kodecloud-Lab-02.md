# Question
### The system admin team at xFusionCorp Industries has streamlined access management by implementing group-based access control. Here's what you need to do:
### a. Create a group named nautilus_admin_users across all App servers within the Stratos Datacenter.
### b. Add the user mohammed into the nautilus_admin_users group on all App servers. If the user doesn't exist, create it as well.
### Note: You can find the infrastructure details by clicking on the Details of all Users and Servers button on the top-right section of the page.

# Solution

stapp01: 172.16.238.10 (tony / Ir0nM@n)
stapp02: 172.16.238.11 (steve / Am3ric@)
stapp03: 172.16.238.12 (banner / BigGr33n)

Let execute the required tasks on all three app servers:
stapp01 (172.16.238.10):
bash -> 
- ssh tony@172.16.238.10
- `Password`: Ir0nM@n
- sudo groupadd nautilus_admin_users
- sudo useradd -G nautilus_admin_users mohammed   `if user doesn't exist`
- `OR if user already exists` :
- sudo usermod -aG nautilus_admin_users mohammed
- grep nautilus_admin_users /etc/group   `verify`
- exit

  ## Note : We will repeat the same for all three server one by one 
