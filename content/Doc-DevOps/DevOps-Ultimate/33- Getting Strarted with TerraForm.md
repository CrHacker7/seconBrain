TerraForm can be downloaded as a single binary or an executable file from the terraForm download section. <www.terraform.io>
## HCL Hashicorp Configuration Language
```java
<block> <parameters> {
	key1 = value1
	key2 = value2
}
```
For exemple: we want to create a file in the local system where TF is installed
1. 
```bash
mkdir /root/terraform-local-file
cd /root/terraform-local-file
```
2.  create a config file called ***local.tf***
```java
resource "local_file" "pet" {
	filename = "/root/pets.txt"
	content = "We love pets!"
}
```
	Block Name: resource 
	resource type:
		local: provider
		file: resources
	Resource Name: pet 
	Arguments: {inside the scope}
	filename/content: they are unmutables for this file

Exemple for creating a EC2
```java
resource "aws_instance" "webserver" {
	ami = "ami-0c2f25c1f55a1ff4d"
	instance_type = "t2.micro"
}
```
Exemple for creating a S3
```java
resource "aws_s3_bucket" "data" {
	bucket = "webserver-bucket-org-2207"
	acl = "private"
}
```

A simple Terraforn workflow consists of four steps:
1. Create the Configuration file
2. Run the init command
3. Review of the execution plan using the Terraform PLAN command
4. Apply the changes using the Terraform APPLY command
```java
terraform init
//this will check the config file and initialize the working directory containing the .tf file.
// It will download the plugin to be able to work on the ressources declared in the .tf file
terraform plan
// This command will show the actions to create the resources
// The output has a + symbol next to the local file type resource called pet. This includes all the args that we specified in .tf
// The + symbol implies the resources will be created
// This step will not create the infrastructure response yet.
terraform apply
// it will display the execution plan once again, it will ask the user to confirm by typing YES to proceed
// To verify, by running CAT command to view the file
terraform show
// within the config directory to see the details of the resource
```
***How do we know what resource types other than local file are available under the provider called local?***
***How do we know what arguments are expected by the local file resource?***
- <https://registry.terraform.io/providers/hashicorp/local/latest/docs>

## Update and Destroy Infrastructure
```java
resource "local_file" "pet" {
	filename = "/root/pets.txt"
	content = "We love pets!"
	file_permission = "0700" //remove any permission except the owner
}
```
console
```java
terraform plan
-/+ // in the beginning of the resource name in PLAN implies that it will be deleted and then recreated
//THIS TYPE OF INFRASTRUCTURE IS CALLED AN IMMUTABLE INFRASTRUCTURE
terraform apply /yes
terraform destroy /yes
```

## LAB
***What is the resource type specified in this file?

***From the terminal, inspect it using tools like cat or text editors such as VI .
Alternatively, you can open this file on VS Code. So navigate to this directory and inspect the file.***
- local_file
***What is the name of the provider for which we are creating this resource?***
- local
```
❯ cat main.tf
resource "local_file" "games" {
  file     = "/root/favorite-games"
  content  = "FIFA 21"
}
```
***If you run a terraform plan now? Would it work?***
- No
- For a newly created configuration, you must run the terraform init first.
***What was the version of the local provider plugin that was downloaded?***
Initializing provider plugins...
- Finding latest version of hashicorp/local...
- Installing hashicorp/local v2.5.2...
- Installed hashicorp/local v2.5.2 (signed by HashiCorp)
```java
root in ~/terraform-projects on ☁️  (us-east-1) 
❯ ls
HCL

root in ~/terraform-projects on ☁️  (us-east-1) 
❯ cd HCL/

root in ~/terraform-projects/HCL via 💠 default on ☁️  (us-east-1) 
❯ LS
-bash: LS: command not found

root in ~/terraform-projects/HCL via 💠 default on ☁️  (us-east-1) 
❯ ls
main.tf

root in ~/terraform-projects/HCL via 💠 default on ☁️  (us-east-1) 
❯ cat main.tf
resource "local_file" "games" {
  file     = "/root/favorite-games"
  content  = "FIFA 21"
}

root in ~/terraform-projects/HCL via 💠 default on ☁️  (us-east-1) 
❯ terraform init

Initializing the backend...

Initializing provider plugins...
- Finding latest version of hashicorp/local...
- Installing hashicorp/local v2.5.2...
- Installed hashicorp/local v2.5.2 (signed by HashiCorp)

Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.

root in ~/terraform-projects/HCL via 💠 default on ☁️  (us-east-1) 
❯ terraform plan
╷
│ Error: Missing required argument
│ 
│   on main.tf line 1, in resource "local_file" "games":
│    1: resource "local_file" "games" {
│ 
│ The argument "filename" is required, but no definition was found.
╵
╷
│ Error: Unsupported argument
│ 
│   on main.tf line 2, in resource "local_file" "games":
│    2:   file     = "/root/favorite-games"
│ 
│ An argument named "file" is not expected here.
╵

root in ~/terraform-projects/HCL via 💠 default on ☁️  (us-east-1) 
❯ vi main.tf

root in ~/terraform-projects/HCL via 💠 default on ☁️  (us-east-1) took 22s 
❯ terraform init

Initializing the backend...

Initializing provider plugins...
- Reusing previous version of hashicorp/local from the dependency lock file
- Using previously-installed hashicorp/local v2.5.2

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.

root in ~/terraform-projects/HCL via 💠 default on ☁️  (us-east-1) 
❯ terraform plan

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # local_file.games will be created
  + resource "local_file" "games" {
      + content              = "FIFA 21"
      + content_base64sha256 = (known after apply)
      + content_base64sha512 = (known after apply)
      + content_md5          = (known after apply)
      + content_sha1         = (known after apply)
      + content_sha256       = (known after apply)
      + content_sha512       = (known after apply)
      + directory_permission = "0777"
      + file_permission      = "0777"
      + filename             = "/root/favorite-games"
      + id                   = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

───────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take
exactly these actions if you run "terraform apply" now.

root in ~/terraform-projects/HCL via 💠 default on ☁️  (us-east-1) 
root in ~/terraform-projects/HCL via 💠 default on ☁️  (us-east-1) 
❯ terraform apply

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # local_file.games will be created
  + resource "local_file" "games" {
      + content              = "FIFA 21"
      + content_base64sha256 = (known after apply)
      + content_base64sha512 = (known after apply)
      + content_md5          = (known after apply)
      + content_sha1         = (known after apply)
      + content_sha256       = (known after apply)
      + content_sha512       = (known after apply)
      + directory_permission = "0777"
      + file_permission      = "0777"
      + filename             = "/root/favorite-games"
      + id                   = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

local_file.games: Creating...
local_file.games: Creation complete after 0s [id=f68b901eb16aff12e9458bdb656a7df8d3425d4c]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

root in ~/terraform-projects/HCL via 💠 default on ☁️  (us-east-1) took 41s 
❯ 
```
***We have now created our very first resource using Terraform! Next, let's work on updating the resource.

***If you look at the output produced by the terraform plan and terraform apply commands closely, we can see that the file content is printed on the screen.
Since we do not want this to happen, we have updated the resource type.
What is the resource type that we have updated?***
- local_sensitive_file (it does not work for me)

***That's right, we have made use of the local_sensitive_file resource type to mask the contents of the file from the execution plan.
However, something is wrong. If we run terraform plan or terraform apply now we see an error!
Identify and fix the issue.
Remember, we don't want the content of the file to show up in the execution plan at all.***
- Delete the line containing the argument called ***sensitive_content*** and then run terraform plan and then terraform apply to re-create the file.
```java
resource "local_sensitive_file" "games" {
  filename     = "/root/favorite-games"
  content  = "FIFA 21"
  sensitive_content = "FIFA 21"
}
```

```java
root in ~/terraform-projects/HCL via 💠 default on ☁️  (us-east-1) took 1m49s 
❯ terraform plan
local_file.games: Refreshing state... [id=f68b901eb16aff12e9458bdb656a7df8d3425d4c]

Terraform used the selected providers to generate the following execution plan.
Resource actions are indicated with the following symbols:
  + create
  - destroy

Terraform will perform the following actions:

  # local_file.games will be destroyed
  # (because local_file.games is not in configuration)
  - resource "local_file" "games" {
      - content              = "FIFA 21" -> null
      - content_base64sha256 = "0QatlfVl9H412mXNB5/Y9evwrDIxEW4ooJpmph2eoUY=" -> null
      - content_base64sha512 = "F0zLI9RS+tFB53xwISp1R3wvRQ/Sw4hsxwysWamsAk/cNyHr/X/pmmykTTCuvkRvr+3y+5c7Sc/J/ObRRX1mIg==" -> null
      - content_md5          = "44a271e06ddd134cdbeab299288422f3" -> null
      - content_sha1         = "f68b901eb16aff12e9458bdb656a7df8d3425d4c" -> null
      - content_sha256       = "d106ad95f565f47e35da65cd079fd8f5ebf0ac3231116e28a09a66a61d9ea146" -> null
      - content_sha512       = "174ccb23d452fad141e77c70212a75477c2f450fd2c3886cc70cac59a9ac024fdc3721ebfd7fe99a6ca44d30aebe446fafedf2fb973b49cfc9fce6d1457d6622" -> null
      - directory_permission = "0777" -> null
      - file_permission      = "0777" -> null
      - filename             = "/root/favorite-games" -> null
      - id                   = "f68b901eb16aff12e9458bdb656a7df8d3425d4c" -> null
    }

  # local_sensitive_file.games will be created
  + resource "local_sensitive_file" "games" {
      + content              = (sensitive value)
      + content_base64sha256 = (known after apply)
      + content_base64sha512 = (known after apply)
      + content_md5          = (known after apply)
      + content_sha1         = (known after apply)
      + content_sha256       = (known after apply)
      + content_sha512       = (known after apply)
      + directory_permission = "0700"
      + file_permission      = "0700"
      + filename             = "/root/favorite-games"
      + id                   = (known after apply)
    }

Plan: 1 to add, 0 to change, 1 to destroy.

────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't
guarantee to take exactly these actions if you run "terraform apply" now.

root in ~/terraform-projects/HCL via 💠 default on ☁️  (us-east-1) 
❯ terraform apply
local_file.games: Refreshing state... [id=f68b901eb16aff12e9458bdb656a7df8d3425d4c]

Terraform used the selected providers to generate the following execution plan.
Resource actions are indicated with the following symbols:
  + create
  - destroy

Terraform will perform the following actions:

  # local_file.games will be destroyed
  # (because local_file.games is not in configuration)
  - resource "local_file" "games" {
      - content              = "FIFA 21" -> null
      - content_base64sha256 = "0QatlfVl9H412mXNB5/Y9evwrDIxEW4ooJpmph2eoUY=" -> null
      - content_base64sha512 = "F0zLI9RS+tFB53xwISp1R3wvRQ/Sw4hsxwysWamsAk/cNyHr/X/pmmykTTCuvkRvr+3y+5c7Sc/J/ObRRX1mIg==" -> null
      - content_md5          = "44a271e06ddd134cdbeab299288422f3" -> null
      - content_sha1         = "f68b901eb16aff12e9458bdb656a7df8d3425d4c" -> null
      - content_sha256       = "d106ad95f565f47e35da65cd079fd8f5ebf0ac3231116e28a09a66a61d9ea146" -> null
      - content_sha512       = "174ccb23d452fad141e77c70212a75477c2f450fd2c3886cc70cac59a9ac024fdc3721ebfd7fe99a6ca44d30aebe446fafedf2fb973b49cfc9fce6d1457d6622" -> null
      - directory_permission = "0777" -> null
      - file_permission      = "0777" -> null
      - filename             = "/root/favorite-games" -> null
      - id                   = "f68b901eb16aff12e9458bdb656a7df8d3425d4c" -> null
    }

  # local_sensitive_file.games will be created
  + resource "local_sensitive_file" "games" {
      + content              = (sensitive value)
      + content_base64sha256 = (known after apply)
      + content_base64sha512 = (known after apply)
      + content_md5          = (known after apply)
      + content_sha1         = (known after apply)
      + content_sha256       = (known after apply)
      + content_sha512       = (known after apply)
      + directory_permission = "0700"
      + file_permission      = "0700"
      + filename             = "/root/favorite-games"
      + id                   = (known after apply)
    }

Plan: 1 to add, 0 to change, 1 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes /"ESCRITO POR MI"
  
local_file.games: Destroying... [id=f68b901eb16aff12e9458bdb656a7df8d3425d4c]
local_file.games: Destruction complete after 0s
local_sensitive_file.games: Creating...
local_sensitive_file.games: Creation complete after 0s [id=f68b901eb16aff12e9458bdb656a7df8d3425d4c]

Apply complete! Resources: 1 added, 0 changed, 1 destroyed.

root in ~/terraform-projects/HCL via 💠 default on ☁️  (us-east-1) took 53s 
❯ 
```

***Notice that the content of the file was not displayed when using local_sensitive_file instead of the local_file resource.
Note: Refer to the documentation to see all the arguments supported by this resource.
Also note that as Terraform follows an immutable infrastructure approach, the file was recreated although the contents are the same.***

***Finally, destroy this resource using terraform destroy.***
```java
❯ terraform destroy
local_sensitive_file.games: Refreshing state... [id=f68b901eb16aff12e9458bdb656a7df8d3425d4c]

Terraform used the selected providers to generate the following execution plan.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # local_sensitive_file.games will be destroyed
  - resource "local_sensitive_file" "games" {
      - content              = (sensitive value) -> null
      - content_base64sha256 = "0QatlfVl9H412mXNB5/Y9evwrDIxEW4ooJpmph2eoUY=" -> null
      - content_base64sha512 = "F0zLI9RS+tFB53xwISp1R3wvRQ/Sw4hsxwysWamsAk/cNyHr/X/pmmykTTCuvkRvr+3y+5c7Sc/J/ObRRX1mIg==" -> null
      - content_md5          = "44a271e06ddd134cdbeab299288422f3" -> null
      - content_sha1         = "f68b901eb16aff12e9458bdb656a7df8d3425d4c" -> null
      - content_sha256       = "d106ad95f565f47e35da65cd079fd8f5ebf0ac3231116e28a09a66a61d9ea146" -> null
      - content_sha512       = "174ccb23d452fad141e77c70212a75477c2f450fd2c3886cc70cac59a9ac024fdc3721ebfd7fe99a6ca44d30aebe446fafedf2fb973b49cfc9fce6d1457d6622" -> null
      - directory_permission = "0700" -> null
      - file_permission      = "0700" -> null
      - filename             = "/root/favorite-games" -> null
      - id                   = "f68b901eb16aff12e9458bdb656a7df8d3425d4c" -> null
    }

Plan: 0 to add, 0 to change, 1 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: "ESCRITO POR MI"
  
local_sensitive_file.games: Destroying... [id=f68b901eb16aff12e9458bdb656a7df8d3425d4c]
local_sensitive_file.games: Destruction complete after 0s

Destroy complete! Resources: 1 destroyed.

root in ~/terraform-projects/HCL via 💠 default on ☁️  (us-east-1) took 29s 
❯ 
```
