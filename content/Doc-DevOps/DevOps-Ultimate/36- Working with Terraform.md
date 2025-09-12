Once we write our configuration file, it's not necessary to run terraform plan or apply to check the syntax used is correct.

> [!tip] `terraform validate`
```java
Success! The configuration is valid.

```

> [!tip] `terraform fmt` (format) 
```java
It scans the code and formats the code (file) into a cononical format
it will be better for reading 
```

> [!tip] `terraform show -json`
```java
It prints the current state of the infrastructure
-json flag : to print the content in JSON format
```

> [!tip] `terraform providers`
```java
To see a list of all providers used
```

> [!tip] `terraform providers mirror /root/terraform/new_local_file`
```java
To copy provide the plugins needed for the current config to another directory.
This will mirror the provider configuration in a new path
```

> [!tip] `terraform output`
```java
content = we love pets!
pet-name = huge_owl
```

> [!tip] `terraform refresh`
```java
It will pick it up and update the state file. It is useful to determine what action to take during the next apply.
IT WILL NOT MODIFY ANY INFRASTRUCTURE, but it will modify the STATE FILE
```

> [!tip] `terraform graph`
```java
It is used to create a visual representation of the dependencies. But it is hard to comprehend and it uses a format called dot
To use this we need to install a software first.

apt update
apt install graphviz -y

Once installed, we can pass the output of the form graph to the dot cmd which we installed using the graph first package and generate a graphic.
```

> [!tip] `terraform graph | dot -Tsvg > graph.svg`
```java
Know, we can open a browser and it should show a dependecy graph (diagram)
```
## LAB

***Which command can be used to create a visual representation of our terraform resources?***
- terraform graph
***there is an error with the configuration.
Use the terraform validate command, troubleshoot, and fix the issue.***
```java
❯ terraform validate
╷
│ Error: Unsupported argument
│ 
│   on main.tf line 8, in resource "tls_private_key" "private_key":
│    8:   dsa_bits  = 2048
│ 
│ An argument named "dsa_bits" is not expected here. Did you mean
│ "rsa_bits"?
╵
```
***The terraform apply failed in spite of our validation working! This is because the validate command only carries out a general verification of the configuration. It validated the resource block and the argument syntax but not the values the arguments expect for a specific resource!***

***The error in the configuration is inside the resource block for the tls_private_key type resource.
It contains the configuration that we needed for generating rsa type key..
Inspect the resource block and fix the issue.
Once done, run terraform plan and then apply to created the resources.***
- Open the Terraform configuration file where the tls_private_key resource is defined.
Locate the tls_private_key resource block.
Remove or comment out the ecdsa_curve argument since it's not needed for RSA keys.
Verify the Changes:

After making the changes, run terraform plan to ensure there are no errors in the configuration.
If everything looks good, proceed with terraform apply to create the resources.
- terraform init, important, then plan and apply
***Fetch details from the state file and identify the value of the filename argument.
Note: Do not rely on the current value in the configuration file.***
- terraform show
***we have used multiple providers so far. But, what are providers?***
- plugins
***we have used multiple providers so far. But, what are providers?***
- mirror
***Now check the provider plugins that have been downloaded from the command line utility (instead of inspecting the .terraform directory). After that choose the correct option.***
```java
❯ terraform providers

Providers required by configuration:
.
├── provider[registry.terraform.io/hashicorp/aws] 4.15.0
└── provider[registry.terraform.io/hashicorp/local]

```

## Mutable vs Inmutable Infrastructure
- When we update the servers in place one by one and with the same way is called MUTABLE INFRASTRUCTURE
- When a server cannot be updated, we destroy it and replace with another one, it is called INMUTABLE INFRASTRUCTURE.
- In IaC, the inmutability  makes it easier to version the infrastructure and to roll back and roll forward between versions.
- Updating the resources block for our local file resource and changing the permission from 777 to 700 will result in the original file to be deleted and a new one created with the updated permission.By default, terraform destroys the resource first before creating a new one in its place.
- If we want it to be created first before the old one is deleted or to ignore deletion. This can be done by making use of lifecycle rules in our resources block.
## Lifecycle Rules

> [!warning] main.tf
``` java
resource "local_file" "pet" {
	filename = "/root/pets.txt"
	content = "We love pets"
	file_permission = "0700"

	lifecycle {
		create_before_destroy = true
	}
}
```
- This rule ensures that when a change in configuration forces the resource to be recreated, a new resource is created first before deleting the old one.

Case when we do not want for any reason the resource to be deleted.
> [!warning] main.tf
``` java
resource "local_file" "pet" {
	filename = "/root/pets.txt"
	content = "We love pets"
	file_permission = "0700"

	lifecycle {
		prevent_destroy = true
	}
}
```
This is specially useful to prevent your resources from getting accidentally deleted. (MYSQL, POSTGRESQL)

##### **Ignore_changes**
This will prevent a resource from being ***updated*** based on a list attributes that we define within the lifecycle block
> [!warning] main.tf
``` java
resource "aws_instance" "webserver" {
	ami = "ami-0dcab43b6fa782257"
	instance_type = "t2.micro"
	tags = {
		Name = "ProjectA-Webserver"
	} 

	lifecycle {
		ignore_changes = [
			tags
		]
	}
}
```
We have asked our form to ignore changes which are made to the tags attribute


> [!warning] main.tf
``` java
resource "aws_instance" "webserver" {
	ami = "ami-0dcab43b6fa782257"
	instance_type = "t2.micro"
	tags = {
		Name = "ProjectA-Webserver"
	} 

	lifecycle {
		ignore_changes = all
	}
}
```
This is specially useful if you don not want the resource to be modified for changes in any resource attributes.

| Order | Option                |                                                     |
| ----- | --------------------- | --------------------------------------------------- |
| 1     | create_before_destroy | Create the resource first and then destroy older    |
| 2     | prevent_destroy       | Prevents destroy of a resource                      |
| 3     | ignore_changes        | Ignore changes to resource attribute (specific/all) |
## LAB
***The main.tf file already has a couple of resource blocks.
Which resource types do they use?***
- local_file && random_string

***Which resource is created first in this case?***
```java
resource "local_file" "file" {
    filename = var.filename
    file_permission =  var.permission
    content = random_string.string.id
}
resource "random_string" "string" {
    length = var.length
    keepers = {
        length = var.length
    }  
}
```
***if you have a resource that uses an attribute from another resource, Terraform will ensure that the resource providing the attribute is created first.*** 
- string
***Let's change the order in which the resource called string is recreated. Update the configuration so that when applied, a new random string is created first before the old one is destroyed.
When ready, apply the changes with terraform apply***
```java
resource "local_file" "file" {
    filename = var.filename
    file_permission =  var.permission
    content = random_string.string.id 
}
resource "random_string" "string" {
    length = var.length
    keepers = {
        length = var.length
    } 
    -----------> 
    lifecycle {
        create_before_destroy = true
    }
    <-----------
}
```

***Read the contents of the file /root/random_text manually. (Try opening with Visual Studio Code / cat command or a text editor)
What is the content of this file?***
- No such file or directory

***Where did the file go?!!?
If you observe the output of the previous apply (scroll up!), you will see that the lifecycle rule we applied caused the local file to the created first and the same file to be destroyed during the recreate operation.
This goes to show that it is not always advisable to use this rule!
In this example, the filename argument for the local_file resource has to be unique which means that we cannot have two instances of the same file created at the same time!
The random_string resource on the other hand is a logical resource that is only recorded in the state and does not have such a restriction.
If you run terraform apply again, the file resource will be created as it does not exist currently.***

***It only contains a single random_pet resource called super_pet.
Under which circumstances will a new pet id be created?***
- change prefix and length
***Now, update the configuration so that the resource super_pet is not destroyed under any circumstances with a terraform destroy or terraform apply command.***
```java
resource "random_pet" "super_pet" {
    length = var.length
    prefix = var.prefix
    lifecycle {
      prevent_destroy = true
    }
}
```

## Datasources

So when we have a file outside of the terraform management, we can access to it by DATA SOURCES.
- Data sources allows terraform to read attributes from resources which are provisioned outside its control

> [!tip] main.tf
```java
resouces "local_file" "pet" {
	filename = "/root/pet.txt"
	content = "data.local_file.dog.content"
}
data "local_file" "dog" {
	filename = "/root/dog.txt"
}
```


| Resource                                 | Data Source                |
| ---------------------------------------- | -------------------------- |
| Keyword: resource                        | Keyword: data              |
| Creates, Updates, Destroy Infrastructure | Only Reads Infrastructure  |
| Also called Managed Resources            | Also called Data Resources |
## LAB
***A data source once created, can be used to create, update, and destroy infrastructure?
True or False?***
- FALSE
***A data source once created, can be used to create, update, and destroy infrastructure?
True or False?***
- TRUE
***A data source block is defined in the main.tf file to read the contents of an existing file.
There is also an output variable that uses reference expression to print the file content using this data source. However, there is something wrong!
Troubleshoot and fix the issue.
When ready, run terraform init, plan and apply to create the datasource. The configuration should print the output variable correctly.***
```java
output "os-version" {
  value = data.local_file.os.content
}
data "local_file" "os" {
  filename = "/etc/os-release"
}
```
***We have now created a new configuration file called ebs.tf within the same configuration directory we have been working on.
What is the resource type that we are working with here?***
- aws_ebs_volume
```java
data "aws_ebs_volume" "gp2_volume" {
  most_recent = true

  filter {
    name   = "volume-type"
    values = ["gp2"]
  }
```
***Once this data source is created, how do we fetch the Volume Id for the resource that is created in AWS?
You may have to look up the documentation for this one. Documentation tab is available at the top right.***
- volume_id
## Meta-Arguments
- If we want to create multiple instances of the same resource.
For exemple: 3 local files
- If we were using a shell script or some other programming language:

> [!tip] create_files.sh
```bash
#!/bin/bash
for i in {1..3}
	do
		touch /root/pet${i}
	done
```
- In terraform is different, these can be done by making use of specific meta arguments in their form.
- We cannot use the same script as it is within the resource block
- Meta arguments can be used within any resource block to change the behavior of resources

> [!warning] *depends_on* 
> for defining explicit dependency between resources

> [!tip] main.tf
```java
resouces "local_file" "pet" {
	filename = var-filename
	content = var.content
	depends_on = [
		random_pet.my-pet
	]
}
resource "random_pet" "my-pet" {
	prefix = var.prefix
	separator = var.separator
	length = var.length
}
```
> [!warning] *lifecycle* 
> how the resources should be created, updated and destroyed within terraform

```java
resouces "local_file" "pet" {
	filename = "/root/pets.txt"
	content = "We love pets"
	file_permission = "0700"
	lifecycle {
		create_before_destroy = true
	}
}
```
## Count (meta-argument)
- Simply add an argument called count with a value greater than one.

> [!tip] main.tf
```java
resouces "local_file" "pet" {
	filename = var-filename

	count = 3
}
```

> [!tip] variables.tf
```java
variable "filename" {
	default = "/root/pets.txt"
}
```

> [!warning] attention
> it won't create 3 uniques files, but 3 times the same ID, so we only have one *pet.txt* created 3 times

The best way to do this is to make use of a list variable for filename. To do this, we have used default values with three elements, each corresponding to the name of the file that we want to create.

> [!tip] main.tf
```java
resouces "local_file" "pet" {
	filename = var-filename[count.index]

	count = 3
}
```

> [!tip] variables.tf
```java
variable "filename" {
	default = [
		"/root/pets.txt"
		"/root/dogs.txt"
		"/root/cats.txt"
	]
}
```

> [!warning] *attention* 
> If we add more elements to the list in the future?
> It will ONLY create 3 because we have set the account to a static value of three.

We want the count to AUTOMATICALLY pick up the number of items that are defined within the filename variable
- to do this, we can set the value of count to use a built in function that would return the length of the list.

> [!tip] main.tf
```java
resouces "local_file" "pet" {
	filename = var-filename[count.index]

	count = length(var.filename)
}
```

> [!tip] variables.tf
```java
variable "filename" {
	default = [
		"/root/pets.txt"
		"/root/dogs.txt"
		"/root/cats.txt"
		"/root/cows.txt"
		"/root/ducks.txt"
	]
}
```
## Length Function
It is used to calculate the size of a list

| variable                                 | function       | value |
| ---------------------------------------- | -------------- | ----- |
| fruits = ["apple", "banana", "orange"]   | length(fruits) | 3     |
| cars = ["honda", "bmw", "nissan", "kia"] | length(cars)   | 4     |
| colors = ["red", "purple"]               | length(colors) | 2     |

> [!NOTE] main.tf
```java
resouces "local_file" "pet" {
	filename = var-filename[count.index]

	count = length(var.filename)
}
output "pets" {
	value = local_file.pet
}

```

> [!NOTE] terraform output
```
pets = [
	{ some data},
	{ some data},
	{ some data},
]
```
If we delete the first element of the list the index will be *decalé* which is an undesirable results when updating this.
## For_each (meta-argument)

> [!NOTE] main.tf
```java
resouces "local_file" "pet" {
	filename = each.value
	for_each = var.filename

}
output "pets" {
	value = local_file.pet
}

```

> [!note] variables.tf
```java
variable "filename" {
	default = [
		"/root/pets.txt"
		"/root/dogs.txt"
		"/root/cats.txt"
	]
}
```

> [!failure] invalid for_each argument
> But this exemple will have an error of invalid argument for_each, It has to be a map or set of strings.

two ways to solve this:
1. 
> [!NOTE] main.tf
```java
resouces "local_file" "pet" {
	filename = each.value
	for_each = var.filename
}
```

> [!note] variables.tf
```java
variable "filename" {
	type = set(string)
	default = [
		"/root/pets.txt"
		"/root/dogs.txt"
		"/root/cats.txt"
	]
}
```
> [!warning] Set
> It cannot contain duplicate elements


2. 
> [!NOTE] main.tf
```java
resouces "local_file" "pet" {
	filename = each.value
	for_each = toset(var.filename)

}
```

> [!note] variables.tf
```java
variable "filename" {
	type = list(string)
	default = [
		"/root/pets.txt"
		"/root/dogs.txt"
		"/root/cats.txt"
	]
}
```
If we delete the first element of the list, and run plan, we can see that only one resource is set to be destroyed, the *"/root/pets.txt".* The other resources will be UNTOUCHED.
- Like this , they are identified by the keys and not by the index, which are file names.
- we can see the results with ***terraform output***

> [!wanting] bypass
> - count = index  (created as a list) 
> - for_each = keys (created as a map)

Other meta arguments in their forms: 
	-provisionals
	-providers
	-backends, etc
## LAB
***The resource local_sensitive_file.name is now created as:***
- LIST
```JAVA
resource "local_sensitive_file" "name" {
    filename = "/root/user-data"
    content = "password: S3cr3tP@ssw0rd"
    
    count = 3

}
```
***How many files creates the Count meta-arg?***
- 1
***Update the main.tf file to make use of the list type variable defined for the filename argument.
Use count to loop through all the elements of this list and do not use hard-coded values.
Use the variable called content for the argument called content.***
```java
resource "local_sensitive_file" "name" {
    filename = var.users[count.index]
    content = var.content
    count = length(var.users)

}
```
EXPLAINATION OF THE CODE
- filename = var.users[count.index]: This sets the filename for each file created by this resource.
var.users is a list variable, and count.index is used to iterate over each element in this list. This means that for each iteration, a different filename from the var.users list is used.
The reason for using var.users instead of var.filename is that var.users is expected to be a list of filenames, allowing the code to dynamically create multiple files based on the list's contents.
content:

- content = var.content: This sets the content of each file to the value of the var.content variable. This variable is expected to hold the content that should be written to each file.
count:

- count = length(var.users): This determines how many instances of the resource should be created. It uses the length function to get the number of elements in the var.users list, ensuring that a file is created for each user in the list.
**Why var.users Instead of var.filename?**
The use of var.users suggests that the variable is a list of filenames, allowing the code to loop through each filename and create a corresponding file. This is more flexible than using a single var.filename, which would imply only one file is being created.
By using count and count.index, the code dynamically handles multiple files, making it scalable and adaptable to changes in the list size.

***A variable called users now has default values added to it.
What type of variable is it?***
- list(string)
```JAVA
variable "users" {
    type = list(string)
    default = [ "/root/user10", "/root/user11", "/root/user12", "/root/user10"]
}
```
***Can the same elements in this list be used as it is for a set instead?***
- NO, there are duplicate elements
***Use for_each to loop through the list type variable called users.
Use the variable called content as the value of the argument content within main.tf.***
```java
resource "local_sensitive_file" "name" {
    filename = each.value
    for_each = toset(var.users)
    content = var.content

}
```
***The resource called name is now created as:***
- map
***The resource address with the filename - /root/user11 is now represented as:***
- local_sensitive_file.name
## Version Constraints

> [!NOTE] main.tf
```java
terraform {
	required_providers {
		local = {
			source = "hashicorp/local"
			version = "1.4.0"
		}
	}
}
resource "local_file" "pet" {
	filename = "/root/pet.txt"
	content = "We love pets!"
}
```

- For getting the version of the providers, go to the official page and in the provider, click version it will drop down a list with all versions.
- then, we will have code block that we can copy and paste within our configuration file.
- We will use of a new block called Terraform, which is used to configure settings related to terraform itself.
- To make use of a specific version of the provider, we need to make use of another block called REQUIRED PROVIDERS inside the terraform block. Inside we can have multiple arguments for every provider that we want to use.
- The value of this argument is an object with the source address of the provider and the exact version we want.

**ANOTHER WAYS TO DO THIS:**
> [!warning] not downloaded
> we asked specifically to not use the version 2.4.0, which download the previous one.

> [!NOTE] main.tf
```JAVA
terraform {
	required_providers {
		local = {
			source = "hashicorp/local"
			version = "!= 2.4.0"
		}
	}
}
resource "local_file" "pet" {
	filename = "/root/pet.txt"
	content = "We love pets!"
}
```

> [!warning] less than (<)
> ***version = "< 2.4.0"***
> To make use of a version less than a given version

> [!NOTE] comparison operators
> ***version = "> 1.2.0, < 2.0.0, != 1.4.0"***
> Combine the comparison operators like this to make use of a specific version within a range.
> We want to make use of any version greater than 1.2.0, but lesser than 2.0.0, but also NOT the version 1.4.0 specifically. 
> Terraform chooses 1.3.0.

> [!warning] Pessimist Constraint Operators
> ***version = "~> 1.2"***
> This operator allows terraform to download the specific version or any available incremental version based on the value we provided.
> This means that terraform can either download the version 1.2 or increamental versions such as 1.3, 1.4, 1.5 all the way up until 1.9

> [!warning] Pessimist Constraint Operators
> ***version = "~> 1.2.0"***
> Terraform can download the version 1.2.0 or the version 1.2.1 or 1.2.2 or the way up until 1.2.9. It will get the latest available.

## LAB
***We have already initialized the configuration directory using the terraform init command.
Inspect the rotation.tf file and find out the correct version of the provider plugin that is downloaded.
Choose the correct version from the below options:***
```java
terraform {
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "> 3.45.0, !=3.46.0, < 3.48.0"
    }
  }
}
```
- 3.47.0
***Which one of the below is not a valid version constraint operator?***
- ==
***Due to a version mismatch, we don't want to download the aws provider version 3.17.0. Which version constraint can be used to achieve this?***
- version = "!=3.17.0"
