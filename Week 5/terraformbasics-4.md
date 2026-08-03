## Terraform Basics I've covered till now 

`Definition` : It's an Iac(Infrastructure as code tool) with having advanced functionality like API as code. It talks with API of cloud providers and create the resource with 
configuration specified in HCL(Hashicorp language) in a terraform script file *main.tf*.

`problem it solved` : Originally there were no concept of hybrid cloud infrastructure because migrating a  part of infra  from one cloud to another takes months and months of energy and resources 
which was not efficient for the organizations. Hence `HashiCorp` came in with `terraform` and said that we don't have to worry about different cloud providers and learning syntax from scratch to
migrate our infra. Just learn this one tool and you can easily migrate between different cloud providers easily. 

`How it works` : Terraform uses a terraform state file as a single source of truth for versioning. It stores the state of the resources created and maintained on cloud  providers. Below is 
the whole process which happens underneath terraform commands ->

**The Condensed Flow**
- init: Terraform reads main.tf, identifies the required cloud (like AWS), and downloads the corresponding Provider Plugin (a translator binary).
- plan: Terraform Core builds a dependency graph of your resources, fetches the current cloud state via the plugin, and calculates the exact differences.
- apply: Terraform Core sends abstract commands to the plugin, which translates them into standard HTTP API calls (like AWS SDK requests) to build the infrastructure.
- State: The cloud returns resource IDs and metadata, which Terraform saves into a terraform.tfstate file to track everything.

` [ main.tf ] ──> [ Terraform Core Engine ] ──(RPC)──> [ AWS Provider Plugin ] ──(REST API)──> [ AWS Cloud ] `

In this process, Remote Procedure Call (RPC) acts as the internal communication bridge between the Terraform Core engine and the AWS Provider Plugin.
Because Terraform Core and the cloud provider plugins are completely separate software programs running on your computer, they cannot talk to each 
other directly through normal memory.

---

## Does terraform only help us to provision cloud infrastructure like aws, cloud, gcp, oracle, DigitalOcean et cetera

- The answer is `No`. until and unless anything which has its own API, We can write terraform provider plugins to manage it.

## Variables

- variables are used in terraform  to prompt value of certain parameters of resources in real time as needs with time may increase or decrease.
- In main.tf file instead of hardcoding the resource we place var.resource_param like `var.Instance_type` and then we give a default value in variables.tf.
- terraform look into it before applying the Infrastructure in an actual cloud provider.
- If no value given to the terraform at runtime after running `terraform apply` then it would take default value and will continue the provisioning.

## Output 
- This is used to extract and display critical infrastructure data after deployment. 
- It is used to help the developers who are restricted from AWS Console to look at what and how many resources are created by the terraform script they ran.
-  While main.tf physically installs a smart lock on your door, output.tf forces the installer to hand you the master passcode and app link so you can actually use it.

`Code Example (main.tf):hcl`
resource "aws_instance" "web" {

  ami           = "ami-0c55b159cbfafe1f0"
  
  instance_type = "t2.micro"

}

`Code Example (output.tf):hcl`
output "server_ip" {

  value       = aws_instance.web.public_ip
  
  description = "The public IP address of the server."
  
}

- The Terminal Result: Running terraform apply instantly prints the data to your screen (e.g., server_ip = "54.213.42.1").
- Instant Visibility: Saves time by revealing data (like URLs or database endpoints) without making you log into the AWS Console to hunt for them.
- Data Sharing: Passes critical resource IDs from one isolated code module or folder to another.
- Security Masking: Allows you to add a sensitive = true tag to hide passwords and private API keys from printing out on public screens.
  

##  Terraform Example 

- visit terraform example in my github or either go to google and type `terraform example github` and you will get bunch of examples to look into. 
