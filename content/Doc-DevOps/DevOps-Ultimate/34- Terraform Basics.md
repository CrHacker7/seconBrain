There are 3 tiers of providers:
1. ***Official providers*** : these are owned and maintained by Hashicorp and include the major cloud providers such as GCP and Azure.
2. ***Verified Providers*** : these are owned and maintained by a third party tecnology company (partner with Hashicorp).
3. ***Community Providers*** : Published and maintained by individual contributors

| Official providers | Verified Providers | Community Providers |
| ------------------ | ------------------ | ------------------- |
| AWS                | Bipip              | ActiveDirectory     |
| Google             | Heroku             | Ucloud              |
| Azure              | Digitalocean       | Netapp-gcp          |
The plugins are downloaded into a hidden directory called **terraform/plugins** in the working directory

## LAB
***We have a new configuration directory located at the path /root/terraform-projects/things-to-do. Inspect this directory and find out the number of providers initialized within this directory.
Do not run terraform init yet!***
- Go to the .terraform/providers directory and count the number of provider plugins installed. If the directory does not exist, there are no plugins downloaded yet.
- 0
```java
root in ~/terraform-projects/things-to-do via 💠 default on ☁️  (us-east-1) 
❯ ls -a
.  ..  main.tf

root in ~/terraform-projects/things-to-do via 💠 default on ☁️  (us-east-1) 
❯ ls -a
.  ..  main.tf  .terraform  .terraform.lock.hcl

root in ~/terraform-projects/things-to-do via 💠 default on ☁️  (us-east-1) 
❯ 
```
***How many configuration files exist in the directory: /root/terraform-projects/things-to-do ?***
- 1
***How many resources are configured in this configuration directory?
Count all the resource blocks used.***
- 2
```java
root in ~/terraform-projects/things-to-do via 💠 default on ☁️  (us-east-1) 
❯ cat main.tf 
resource "local_file" "things-to-do" {
  filename     = "/root/things-to-do.txt"
  content  = "Clean my room before Christmas\nComplete the CKA Certification!"
}
resource "local_file" "more-things-to-do" {
  filename     = "/root/more-things-to-do.txt"
  content  = "Learn how to play Astronomia on the guitar!"
}
root in ~/terraform-projects/things-to-do via 💠 default on ☁️  (us-east-1) 
❯ 
```
***Now, go ahead and create these resources using terraform!
Once done, the two files defined inside the resource blocks should be created with the correct file names and content.***
- terraform plan
- terraform apply
***How many resources are configured within this configuration directory?
Make sure to check all the .tf files.***
- 2

***Create a new configuration file within the same directory called xbox.tf. This file should make use of the same local_file resource type with the below requirements:
	Resource Name: xbox
	filename: /root/xbox.txt
	content: Wouldn't mind an XBox either!
Once the configuration file has been created, use the ==terraform workflow== to create this resource.
- NO spaces in the Resource Name
- terraform init, terraform plan, terraform apply.

***Now, navigate to the directory /root/terraform-projects/provider-a. We have downloaded a plugin in this directory. Identify the name and type of provider.
If the configuration files in this directory seem unfamiliar, do not worry, these are covered later in the course.***
- Run terraform init or inspect the .terraform directory. Also, make use of the documentation to determine the type of provider used.
- Partner-linode (PARA SABER MIRAR LA DOC)

***Now, navigate to the directory /root/terraform-projects/provider-b. We have downloaded a plugin in this directory. Identify the name and type of provider.
If the configuration files in this directory seem unfamiliar, do not worry, these are covered later in the course.***
- terraform init (output the providers)
- community- ansible

> [!NOTE] check the providers
> The best and fastest way to check the providers' type is the documentation!

## Configuration Directory
- We can have many config file or one ***main*** that contains all the resource blocks to provision the infrastructure.
- A single configuration file can have as many number of configuration blocks that we need
- Other configuration files we can create:
	- varialbe.tf : Contains variable declarations
	- output.tf : Contains outputs from resources
	- provider.tf : Contains Provider definition
## Multiple Providers
- random provider (local & random)
- main.tf
```java
resource "local_file" "pet" {
	filename = "/root/pets.txt"
	content = "We love pets!"
}
resource "random_pet" "my-pet" {
	prefix = "Mrs"
	separator = "."
	length = "1"
}
```
- the keyword before the underscore is the ***provider***, which in this case is ***random***
- the kw following it is the ***resource type***, which is the ***pet***
- The resource name is ***my-pet***
- terraform workflow ***init-plan-apply***

> [!note] Nota 
> Only will create one, random provider, because the local provider was created previously.

## LAB


> [!tip] Provider existence
> if we have only the .tf file in our ***directory***
> it means we do not have any providers initialized

if we do not have ***.terraform/provider*** it is not been initialized it
```java
resource "local_file" "my-pet" {
     filename = "/root/pet-name"
     content = "My pet is called finnegan!!"

}
resource "random_pet" "other-pet" {
     length = "1"
     prefix = "Mr"
     separator = "."

}
```
***This is because whenever we add a resource for a provider that has not been used so far in the configuration directory, we have to initialize the directory by running terraform init command.
Let's do that now. Run terraform init followed by terraform apply command.***
- terraform init, and apply

## Using Input Variables
- variables.tf
```java
variable "filename" {
	default = "/root/pets.txt"
}
variable "content" {
	default = "We love pets!"
}
variable "prefix" {
	default = "Mrs"
}
variable "separator" {
	default = "."
}
variable "length" {
	default = "1"
}
```
- keyword "variable" : variable
- variable name : the args names of the blocks
***How can we use it?***
- main.tf
```java
resource "local_file" "pet" {
     filename = var.filename
     content = var.content

}
resource "random_pet" "my-pet" {
     prefix = var.prefix
     separator = var.separator
     length = var.length

}
```

> [!note] modify args
> If we change the args we do not need to do the whole worflow, just the ***apply***

## Understanding the Variable Block
- variable.tf
```java
variable "filename" {
	default = "/root/pets.txt"
	type = string
	description = "the path of local file"
}
variable "content" {
	default = "We love pets!"
	type = string
	description = "the content of the file"
}
variable "prefix" {
	default = ["Mrs", "Mrs", "Sir"]
	type = list
	description = "the prefix to be set"
}
variable "separator" {
	default = "."
	type = string
	description = "the path of local file"
}
variable "length" {
	default = 2
	type = number
	description = "the length of the pet name"
}
variable "password_change" {
	default = true
	type = bool
}

```
- main.tf
```java
resource "random_pet" "my-pet" {
     prefix = var.prefix[0]
     separator = var.separator
     length = var.length

}
```

| Type   | Example                 |
| ------ | ----------------------- |
| string | "/root/pets.txt"        |
| number | 1                       |
| bool   | true/false              |
| any    | Default value           |
| list   | ["cat", "dog"]          |
| map    | pet1 = cat / pet2 = dog |
| object | Complex Data Structure  |
| tuple  | Complex Data Structure  |

> [!note] nota
> If it is not specified in the variable block, it is set to the type any by default
```java
variable "file-content" {
	type = map
	default = {
		"statement1" = "We love pets!"
		"statement1" = "We love animals!"
	}
}
```
- main.tf
```java
resource "local_file" "my-pet" {
	filename = "/root/pets.txt"
	content = var.file-content["statement2"]
}
```
####  List of type
```java
variable "prefix" {
	default = ["Mr", "Mrs", "Sir"]
	type = list(string)
}
// list number
variable "prefix" {
	default = [1, 2, 3]
	type = list(number)
}
```

#### Map of a type
```java
variable "cats" {
	default = {
		"color" = "brown"
		"name" = "bella"
	}
	type = map(string)
}
// map number
variable "pet_count" {
	default = {
		"dogs" = 3
		"cats" = 1
		"goldfish" = 2
	}
	type = map(number)
}
```
#### Set

> [!NOTE] List vs Set
>  the difference between a set and a list is that a set cannot have ***duplicate*** elements

```java
variable "prefix" {
	default = ["Mr", "Mrs", "Sir", "Mr"] //wrong duplicated
	type = set(string)
}
variable "age" {
	default = [10, 12, 15, 10] //wrong double 10
	type = set(number)
}
```
#### Objects
```java
variable "bella" {
	type = object({
		name = string
		color = string
		age = number
		food = list(string)
		favorite_pet = bool
	})
	default = {
		name = "bella"
		color = "brown"
		age = 7
		food = ["fish", "chicken", "turkey"]
		favorite_pet = true
	}
}
```
#### Tuples
It's similar to a list and consists of a sequence of elements

> [!NOTE] List vs Tuples
> list uses elements of the same variable type, but in case of tuple, we can make use of elements of different variable types.
> The variables to be passed to this should exactly be in number, and of that specific type for it.
```java
variable "kitty" {
	type = tuple([string, number, bool])
	default = ["cat", 7, true] 
	
}
```

## Using Variables in TerraForm
- Command Line Flags
```java
terraform apply -var "filename=/root/pets.txt" -var "content=We love Pets!" -var "prefix=Mrs" -var "separator=." -var "length=2"
```
- Environment Variables
```java
export TF_VAR_filename="/root/pets.txt"
export TF_VAR_content="We love Pets!" 
export TF_VAR_prefix="Mrs" 
export TF_VAR_separator="." 
export TF_VAR_length="2"
terraform apply 
```
- Variable Definition Files
	- terraform.tfvars
```java
filename = "/root/pets.txt"
content = "We love Pets!" 
prefix = "Mrs" 
separator = "." 
length = "2"
// then in console $ terraform apply
```

> [!NOTE] Names we can use to name the file
> - `terraform.tfvars`
> - `terraform.tfvars.json`
> 
> 	Automatically loaded
> - `*.auto.tfvars`
> - `*.auto.tfvars.json`
> 
> 	 You passed it in the cmd by -var-file:
> - `terraform apply -var-file variables.tfvars`

- Variables Definition Precedence
```java
++++main.tf+++++
-resource local_file pet {
	filename = var.filename
}
________________________________________
++++variables.tf++++
-variable filename {
	type = string
}
```


```java
________________________________________
+++++bash+++++
-$ export TF_VAR_filename="/root/pets.txt"
________________________________________
terraform.tfvars
-filename = "/root/pets.txt"
________________________________________
-variable.auto.tfvars
filename = "/root/mypet.txt"
________________________________________
+++++bash+++++
terraform apply -var "filename=/root/best-pet.txt"
```
***which one of these values would be accepted?*** [DOCS](https://developer.hashicorp.com/terraform/language/values/variables)

| Order | Option                                  | Priority         |
| ----- | --------------------------------------- | ---------------- |
| 1     | Env Variables                           |                  |
| 2     | terraform.tfvars                        |                  |
| 3     | *.auto.tfvars (alphabetical order)      |                  |
| 4     | -var or  -var-file (command line flags) | HIGHEST PRIORITY |
>[!warning] So it will overwrite any of the previous values

## LAB
***How can we use environment variables to pass input variables in terraform scripts?***
- Export variables using the prefix TF_VAR_ followed by the variable name and a value.
***Which method has the highest priority in Variable Definition Precedence?
If unsure, Refer to the documentation. Documentation tab is available at the top right.***
- Command line flag `terraform apply -var-file variables.tfvars`

***Which one of the following commands is a valid way to make use of a custom variable file with the terraform apply command?***
- `terraform apply -var-file variables.tfvars`

***The terraform plan command did not run as there was no reference for the input variable called filename in the configuration files.
Let's fix that now.***
```java
resource "local_file" "games" {
  filename = var.filename
  content = "football"
}
!!!!!ERROR!!!!!
Error: Reference to undeclared input variable
│ 
│   on main.tf line 2, in resource "local_file" "games":
│    2:   filename = var.filename
│ 
│ An input variable with the name "filename" has not been declared. This variable can be
│ declared with a variable "filename" {} block.
╵
```
***Declare the variable called filename with type string in the file variables.tf.
Don't have to specify a default value.***
```java
Solution available for variables.tf file :-

variable filename {
  type = string
}
```
***If we run terraform apply with a -var command line flag as shown below, which value would be considered by terraform?
terraform apply -var filename=/root/tennis.txt***
- `/root/tennis.txt`
***Terraform follows a variable definition precedence order to determine the value and
the command line flag of –var or –var-file takes the highest priority.***

## Resource Attribute Reference
```java
resource "local_file" "pet" {
	filename = var.filename
	content = "My fav pet is ${random_pet.my-pet.id}"
}
resource "random_pet" "my-pet" {
	prefix = var.prefix
	separator = var.separator
	length = var.length
}
```
- This expression is used to reference the attribute from the resource called my-pet

> [!warning] Sintax
> $ { Resource Type + Resource Name + Attribute }
> All are separated by a period or dot, NOTHING HAS SPACE.

## LAB
***Practice as if you are the worst, Perform as if you are the best.***
- The resource_type used is time_static from the provider called time.
	resource "time_static" "time_update" {
	}
**As you can see, the resource block is empty. This is because time_static does not need any arguments to be supplied to work.
When applied as it is, terraform creates a logical resource locally (similar to random_pet) with the current time.**

***Which of the following attributes are exported by the time_static resource?***
- Attributes are value storing fields in resource, data source, or provider schemas. Every attribute has an associated value type, which describes the kind of data the attribute can hold. Attributes also can describe value plan modifiers (resources only) and value validators in addition to those defined by the value type.
Generally, **attribute references** are information related to the infrastructure we are configuring. **==We can obtain them only after our code is applied.==**
The following commands, when executed in sequence (after init), can display the attributes that will be exported:
terraform plan
The value of the attribute is only ==accessible after apply.==
```java
Terraform used the selected providers to generate the
following execution plan. Resource actions are indicated with
the following symbols:
  + create

Terraform will perform the following actions:

  # time_static.time_update will be created
  + resource "time_static" "time_update" {
      + day     = (known after apply)
      + hour    = (known after apply)
      + id      = (known after apply)
      + minute  = (known after apply)
      + month   = (known after apply)
      + rfc3339 = (known after apply)
      + second  = (known after apply)
      + unix    = (known after apply)
      + year    = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

──────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so
Terraform can't guarantee to take exactly these actions if you
run "terraform apply" now.
```
The above command gives a preview of all the attributes that will be exported.
To view the attributes actually exported by the resource, run terraform apply followed by terraform show:
```java
# time_static.time_update:
resource "time_static" "time_update" {
    day     = 29
    hour    = 12
    id      = "2024-01-29T12:46:34Z"
    minute  = 46
    month   = 1
    rfc3339 = "2024-01-29T12:46:34Z"
    second  = 34
    unix    = 1706532394
    year    = 2024
}
```
The resource exports an attribute called id. We can see the attributes for the resource once its created by using terraform commands, we will see them in the upcoming lectures.

****How do we refer to the attribute called id using a reference expression?***
- The syntax of the reference is `resource_type.resource_name.attribute.`
- `time_static.time_update.id`
Now, update the main.tf file and add a new local_file resource called time with the following requirements:
filename: /root/time.txt 
content: Time stamp of this file is `<id from time_update resource>`
Use a reference expression and interpolation.

```java
resource "local_file" "time" {
  filename = "/root/time.txt"
  content = "Time stamp of this file is ${time_static.time_update.id}"
 }
 resource "time_static" "time_update" {
}
```
***What is the attribute called rfc3339 that is created for the time_static resource called time_update?
Make use of the terraform show command and identify the attribute values.***
- `terraform show`
-  "2025-01-16T16:34:08Z"
```java
❯ terraform show
# local_file.time:
resource "local_file" "time" {
    content              = "Time stamp of this file is 2025-01-16T16:34:08Z"
    content_base64sha256 = "BKCw9fhziWI9Y+QGd1C1bk3uSvWx6wj3r8gQQ0RZ08A="
    content_base64sha512 = "NPtQpDcC2EO22owuH1o/4ZVCQUZMC9ViTyWT9uXftn+iBYx2j8pesn/3ENHz1IwI2nrFOz2/TQSuQguLnlt5WA=="
    content_md5          = "c8264869f045496b016b8c9b2ec937da"
    content_sha1         = "31b997b36f3bc03cbb6a3fe51f61406fa90548b1"
    content_sha256       = "04a0b0f5f87389623d63e4067750b56e4dee4af5b1eb08f7afc810434459d3c0"
    content_sha512       = "34fb50a43702d843b6da8c2e1f5a3fe1954241464c0bd5624f2593f6e5dfb67fa2058c768fca5eb27ff710d1f3d48c08da7ac53b3dbf4d04ae420b8b9e5b7958"
    directory_permission = "0777"
    file_permission      = "0777"
    filename             = "/root/time.txt"
    id                   = "31b997b36f3bc03cbb6a3fe51f61406fa90548b1"
}

# time_static.time_update:
resource "time_static" "time_update" {
    day     = 16
    hour    = 16
    id      = "2025-01-16T16:34:08Z"
    minute  = 34
    month   = 1
    rfc3339 = "2025-01-16T16:34:08Z"
    second  = 8
    unix    = 1737045248
    year    = 2025
}
```
## Resource Dependencies
#### Implicit Dependency
- We saw how to link one resource to another using Reference Attribute by making use of Reference expression and Interpolation.
- We were able to make use of the output of the random_pet resource as an input for the local file resource.
- First, Terraform creates the random_pet resource and then it creates the local_file resource.
- When resources are deleted, Terraform deletes it in the reverse order, the local_file first and then the random_pet.

> [!warning] Implicit Dependency 
> - The local_file resource depends on the output of the random_pet resource.
> 

#### Explicit Dependency
- We can configure the order of the dependency by modifying the file.
```java
resource "local_file" "pet" {
	filename = var.filename
	content = "My fav pet is Mr. Cat"
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
- This will ensure that the local_file is only created after the random_pet resource created.
> [!warning] Type of Dependency
> This type of dependecy is called ***Explicit Dependency***.
> - It is only necessary when a resource relies on some other resource indirectly.
> - And it does not make use of a reference expression as seen in this case.

***
## LAB
***Which argument should be used to explicitly set dependencies for a resource?***
- depends_on
***Resource A relies on another Resource B but doesn't access any of its attributes in its own arguments. What is this type of dependency called?***
- Explicit dependency
***Resource A relies on another Resource B but doesn't access any of its attributes in its own arguments. What is this type of dependency called?***
- When we use reference expressions to link resources, the dependency created is called implicit dependency
- Reference expression
***In the configuration directory /root/terraform-projects/key-generator, create a file called key.tf with the following specifications:
Resource Type: tls_private_key
Resource Name: pvtkey
algorithm: RSA
rsa_bits: 4096
When ready, run terraform init, plan and apply.***
```java
resource "tls_private_key" "pvtkey" {
    algorithm   = "RSA"
    rsa_bits = 4096
}
```
*Resource tls_private_key generates a secure private key and encodes it as PEM. It is a logical resource that lives only in the terraform state.
You can see the details of the resource, including the private key by running the terraform show command.
You can read the documentation for more details. https://registry.terraform.io/providers/hashicorp/tls/latest/docs/resources/private_key*

***Now, let's use the private key created by this resource in another resource of type local file. Update the key.tf file with the requirements:
	Resource Name: key_details
	File Name: /root/key.txt
	Content: use a reference expression to use the attribute called private_key_pem of the pvtkey resource.
When ready, run terraform init, plan and apply.***
```java
resource "tls_private_key" "pvtkey" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

resource "local_file" "key_details" {
  content  = tls_private_key.pvtkey.private_key_pem
  filename = "/root/key.txt"
}
```
***Now destroy these two resources.***
- `terraform destroy`
- 
***Within this directory, create two local_file type resources in main.tf file.
	Resource 1:
	Resource Name: whale
	File Name: /root/whale
	content: whale
	Resource 2:
	Resource Name: krill
	File Name: /root/krill
	content: krill
Resource called whale should depend on krill but do not use reference expressions.
When ready, run terraform init, plan and apply.***
```java
resource "local_file" "whale" {
  filename   = "/root/whale"
  content    = "whale"
  depends_on = [local_file.krill]
}
resource "local_file" "krill" {
  filename = "/root/krill"
  content  = "krill"
}
```
***
## Output Variables
- When we apply the configuration to save this ID in an output variable called pet-name, we can create an output block

> [!tip]  main.tf
```java
resource "local_file" "pet" {
	filename = var.filename
	content = "My fav pet is ${random_pet.my-pet.id}"
}
resource "random_pet" "my-pet" {
	prefix = var.prefix
	separator = var.separator
	length = var.length
}
output "pet-name" {
	value = random_pet.my-pet.id
	description = "Record the value of pet ID generated by the random_pet resource"
}
```

> [!tip] variables.tf
```java
variable "filename" {
	default = "/root/pets.txt"
}
variable "content" {
	default = "We love pets!"
}
variable "prefix" {
	default = "Mrs"
}
variable "separator" {
	default = "."
}
variable "length" {
	default = "1"
}
```

> [!tip] sintax Output
>
```java
 output "<variable_name>" {
> 	value = "<variable_value>"
> 	<arguments>
> }
```
> [!warning] sintax
> The *mandatory* argument for **value** is the Reference Expression.
> The description is an optional arg to describe what is used for.
> 

- When we run terraform apply, we can see that the output variable is printed on the screen.
- We can use `terraform output` too. It'll print all the output variables defined in all the files of that directory.
- `terraform output pet-name` to print an existing variable output.
- The best use of this is when you want to quickly display details about a provision resource on the screen or to feed the output variable to other easy tools such as an add on script or ansible for configuration or testing.
___
## LAB
***Which provider is used by the configuration files in this directory?***
- random
***Which two resource types are configured in the configuration files?***
- random_uuid & random _integer
***What is the value of the output variable called order1 ?
Use the terraform output command.***
```java
root in ~/terraform-projects/data via 💠 default on ☁️  (us-east-1) 
❯ terraform output
id1 = "4e7ed420-3b87-1d9f-ce50-7bc1c7cc8557"
id2 = "bf0e4513-c09d-ae98-d31c-8458f3110684"
id3 = "b0e0819b-776a-673c-84b1-64bc228279e0"
id4 = "f6af75de-21a8-214d-26ae-e03b78ea4bde"
id5 = "13e352f7-4846-5a50-78d4-05c9e57c764a"
id6 = "2d959da1-4b5e-c695-3a93-84835bc556c0"
id7 = "525bf56b-f747-5844-d473-7b606695884f"
order1 = 99602
order2 = 99602

root in ~/terraform-projects/data via 💠 default on ☁️  (us-east-1) 
❯ terraform output order1
99602

```
***What is the value of the output variable pet-name ?***
```java
root in ~/terraform-projects/output via 💠 default on ☁️  (us-east-1) 
❯ terraform output
pet-name = "bass"
```
***We have just updated the main.tf file in this directory with a new resource block.
Add a new output variable with the following specifications:
	Output Variable Name: welcome_message
	Value: content of the resource called welcome
When ready, run terraform init, plan and apply***
```java
resource "random_pet" "my-pet" {
  length = var.length
}

output "pet-name" {
  value       = random_pet.my-pet.id
  description = "Record the value of pet ID generated by the random_pet resource"
}

resource "local_file" "welcome" {
  filename = "/root/message.txt"
  content  = "Welcome to Kodekloud."
}
output "welcome_message" {
  value = local_file.welcome.content //DON'T FORGET THE ARG!!
}
```
