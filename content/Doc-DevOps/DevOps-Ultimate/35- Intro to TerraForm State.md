Let's the Terraform workflows again:
`ls terraform-local-file`
We have a directory called terraform-local-file

> [!NOTE]  |>_
```bash
ls terraform-local-file
main.tf variable.tf
```

> [!NOTE] main.tf
```java
resource "local_file" "pet" {
	filename = var.filename
	content = var.content
}
```


> [!NOTE] variables.tf
```
variable "filename" {
	default = "/root/pets.txt"
}
variable "content" {
	default = "I Love pets!"
}
```

1. We'll run ***terraform init*** command to download the necessary plugin.
2. We can generate the execution plan with ***terraform plan***
	1. the very first line that is printed tries to refresh the state in memory prior to the plan. If it's the first time running this cmd, there will be no details related to state printed. Then it created.
3. We'll run ***terraform apply*** to refresh the in memory state, finds that there is no state recorded at the moment and then proceeds to create an execution plan. Once confirmed, it creates the local file resource. Terraform creates a unique ID for the resource.
	1. If we run it again, it knows that a local file resource by the name of PET and the same ID as before, it takes no further action.
So, terraform knows the 3.1 step cause in the directory the is an additional file called ***terraform state*** created.

> [!NOTE]  |>_
```bash
ls terraform-local-file
main.tf variable.tf ==terraform.tfstate==
```
- it is called ***Terraform state file*** which was created from **apply** cmd. that created the resource in the first place.
- The state file is a JSON data structure. Inside has a complete record of the infrastructure created by terraform.
- When we modify anything, this file will be compare with the new one, and will make the updates, it'll change the ID too.
- So at this point the **configuration file** and the **state file** are in sync.
## Purpose of the State
- In a real workd will be multiples resources and providers, this mechanism is not recommended, it is not possible for terraform to reconcile state for every operation cause it will takes several minutes.
- For avoiding terraform to go slow, we can pass a flag:
- `terraform plan --refresh=false`
- it relies on the cached attributes.
When we work in group it is better to have one source, one remote data store for the terraform files. Like this we ensure to have the latest version of the state, only one person at a time. We can use *AWS S3, Google Cloud Storage, Terraform Cloud, HashiCorp Consul.*

## Terraform State Consideration
- The state file contains sensitive info.
- We have to ensure the state file is always stored in as secure storage.
- We have two kinds of files in our config directory data: the terraform configuration  that we use to provision and manage infrastructure and the state of the infrastructure.
- Owing to the sensitive nature of the state file, It is not recommended to store them in git repositories. Instead, store the state in remote backend systems such as AWS S3, GCS, AZURE, HASHICORP CLOUD.

> [!WARNING] 
> - Never manually attempt to edit the state files ourselves. It's better to do it with terraform commands

## LAB
***Which location is the terraform state file stored by default?***
- inside the configuration directory
```java
root in ~/terraform-projects/project-flash via 💠 default on ☁️  (us-east-1) 
❯ ls
main.tf  reverse-flash.tf  riddler.tf  zoom.tf
```
***Which option should we use to disable state?***
- we cannot disabled state!
***Which format is the state file stored in by default?***
- json
***Which format is the state file stored in by default?***
- terraform init
***What is the name of the state file that is created by default?***
- terraform.tfstate
***Navigate to the configuration directory /root/terraform-projects/project-flash we have created a few configuration files here. The directory has been initialized and the provider plugins downloaded inside the .terraform directory. However, there is no terraform.tfstate file created. Why is that?***
- terraform apply was not run
***Run the terraform show command and identify the id created for the resource called speed_force.***
- terraform init, plan, apply
***Run the terraform show command and identify the id created for the resource called speed_force.***
- there is not state
***Now, check terraform show again. What is the value of id for the resource called speed_force?***
```java
- # local_file.speed_force:
resource "local_file" "speed_force" {
    content              = "speed-force"
    content_base64sha256 = "+hI5F86aVJG7nQ6K0VEOJHTIhlj5aRLnpODNbyZExtI="
    content_base64sha512 = "COfaah4Goo2T1qerQ8gYg5uR6onGpW1IjlpCtZuOW3UT+MH0rzPSj/LSKTJHHCfYVL0w3Q0B78u8RsRpueUNqg=="
    content_md5          = "b5db1e5be7170beefea11ae7271a06a8"
    content_sha1         = "ebeb8b595c8eb4a6e81cacf244146e742fab2981"
    content_sha256       = "fa123917ce9a5491bb9d0e8ad1510e2474c88658f96912e7a4e0cd6f2644c6d2"
    content_sha512       = "08e7da6a1e06a28d93d6a7ab43c818839b91ea89c6a56d488e5a42b59b8e5b7513f8c1f4af33d28ff2d22932471c27d854bd30dd0d01efcbbc46c469b9e50daa"
    directory_permission = "0777"
    file_permission      = "0777"
    filename             = "/root/speed-force"
    id                   = "ebeb8b595c8eb4a6e81cacf244146e742fab2981"
}
```
***Among them is an EC2 Instance which is created by the resource called dev-server. See if you can find out the private_ip for the instance that was created.***
```
private_ip = "10.167.12.12"
```