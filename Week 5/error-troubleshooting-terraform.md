#  Terraform Deep Dive: State Drift, API Validation, & The AWS Provider

**Objective:** To understand how Terraform handles manual infrastructure changes (drift) versus configuration errors, and to map out exactly how the Terraform CLI interacts with the AWS API.

To learn this, I intentionally broke my configuration and manually altered running AWS infrastructure to see how Terraform responds.

---

## 🛠️ Scenario 1: Forcing State Drift

**The Experiment:** I went into the AWS Console and manually forced an EC2 instance into a `stopped` state, even though Terraform had originally provisioned it to be running. 

**The Result:** 
Because the real-world infrastructure changed without telling Terraform, the state file no longer matched reality. This is **State Drift**.

*   **During `terraform plan`:** Terraform's AWS Provider checked the current status of the AWS resources, saw the instance was stopped, and flagged the mismatch. It proposed an update to change the state from `stopped` back to `running`.
*   **During `terraform apply`:** Terraform actively "healed" the infrastructure by sending a request to the AWS API to start the instance, bringing reality back in line with my configuration code.

---

## 🛑 Scenario 2: Inducing a Real Error

**The Experiment:** I changed the `instance_type` in my `aws_instance` resource from `t2.micro` to `not-a-real-type` to see how Terraform handles invalid parameters.

**The Result:**
Surprisingly, `terraform plan` succeeded, but `terraform apply` failed with a hard error. 

```text
╷
│ Error: updating EC2 Instance (i-03623550b224fb56f) type: modifying EC2 Instance (i-03623550b224fb56f) InstanceType (not-a-real-type) attribute: operation error EC2: ModifyInstanceAttribute, https response error StatusCode: 400, RequestID: 6fe5d57b-2e1d-40d1-9ec9-f46574a8f295, api error InvalidParameterValue: The following supplied instance types do not exist: [not-a-real-type]
│
│   with aws_instance.app_server,
│   on main.tf line 16, in resource "aws_instance" "app_server":
│
16: resource "aws_instance" "app_server" {
│  a@DESKTOP-CRG1TC1 MINGW64 ~/OneDrive/Documents/projects/personal-projects/write_your_first_terraform_project/aws/local_state (main)
$ terraform plan

Planning failed. Terraform encountered an error while generating this plan.

╷
│ Error: invalid AWS Region: not-a-real-region
│
│   with provider["registry.terraform.io/hashicorp/aws"],
│   on main.tf line 12, in provider "aws":
│   12: provider "aws" {
│
╵

