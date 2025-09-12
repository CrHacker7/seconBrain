#### 1. Q: How many Ansible plays are there in the following given playbook?
```js
---
- name: Setup apache
  hosts: webserver
  tasks:
    - name: install httpd
      yum:
        name: httpd
        state: installed
    - name: Start service
      service:
        name: httpd
        state: started

- name: Setup tomcat
  hosts: appserver
  tasks:
    - name: install httpd
      yum:
        name: tomcat
        state: installed
    - name: Start service
      service:
        name: tomcat
        state: started
```
**A:** 2
#### 2. Q: How many tasks are there under Setup apache Ansible play?
```js
- name: Setup apache
  hosts: webserver
  tasks:
    - name: install httpd
      yum:
        name: httpd
        state: installed
    - name: Start service
      service:
        name: httpd
        state: started

- name: Setup tomcat
  hosts: appserver
  tasks:
    - name: install httpd
      yum:
        name: tomcat
        state: installed
    - name: Start service
      service:
        name: tomcat
        state: started
```
**A:** 2

#### 3. Q: If we use the following inventory, on which hosts will Ansible install the httpd package using the given playbook?
```json
[webserver]
web1
web2
[appserver]
app1
app2
app3

---
- name: Setup apache
  hosts: webserver
  tasks:
    - name: install httpd
      yum:
        name: httpd
        state: installed

- name: Setup tomcat
  hosts: appserver
  tasks:
    - name: install httpd
      yum:
        name: tomcat
        state: installed

```
**A:** The httpd package is being installed by Setup apache play, which is supposed to run for webserver hosts. As per the given inventory, webserver group contains two hosts, web1 and web2, so the playbook will install httpd package on web1 and web2 hosts. ==WEB1 AND WEB2==

#### 4. Q: Which of the following commands can you use to run an Ansible playbook named install.yaml?

==(A) ansible-playbook install.yaml==
(B) ansible-playbook run install.yaml
(C) ansible-playbook -p install.yaml
(D) ansible-playbook -i install.yaml

**A:** **ansible-playbook install.yaml** command can be used to run install.yaml playbook. 
### 5. Q: A sample playbook named update_service.yml is shown below, it is supposed to update a service on your servers.
```js
- hosts: all
  tasks:
    - name: Install a new package
      apt:
        name: new_package
        state: present

    - name: Update the service
      service:
        name: my_service
        state: restarted

    - name: Check service status
      service:
        name: my_service
        state: started
```

Which command would you use to run update_service.yml playbook in check mode?
A. ansible update_service.yml
B. ansible-playbook update_service.yml --check
C. ansible-playbook update_service.yml
D. ansible-playbook --check update_service.yml
**A:** To run update_service.yml playbook in check mode, use ansible-playbook **update_service.yml --check** command.
A. ansible update_service.yml
==B. ansible-playbook update_service.yml --check==
C. ansible-playbook update_service.yml
D. ansible-playbook --check update_service.yml
#### 6. Q: Consider again the same sample playbook named update_service.yml as shown below.
```js
- hosts: all
  tasks:
    - name: Install a new package
      apt:
        name: new_package
        state: present

    - name: Update the service
      service:
        name: my_service
        state: restarted

    - name: Check service status
      service:
        name: my_service
        state: started
```
**A:** Let's suppose you have already ran this playbook on your server. Now, once you run this playbook in check mode against same server, which tasks would result in changed status?
A. Install a new package
B. Update the service
C. Check service status
D. All of the tasks
**A:** In the provided sample playbook update_service.yml, there are three tasks:
A. Install a new package | B. Update the service | C. Check service status

When u*pdate_service.yml* playbook is already ran on a server and we try to run it again in check mode against same server, Ansible will indicate which tasks would result in changed status.
The task *Install a new package* would not be marked as changed because it's just ensuring the package is present and that was already installed on the server from earlier execution(s).
The task *Update the service* would be marked as changed because restarting a service is a change in state.
The task *Check service status* would not be marked as changed since it's only verifying the service's state, and previous task has already started it.
Therefore, the tasks that would result in changed status is: **Update the service**
#### 7. Q: There is another sample playbook named configure_database.yml that modifies a configuration file on your database servers. The initial code sample is as follows:
```js
- hosts: all
  tasks:
    - name: Set max connections
      lineinfile:
        path: /etc/postgresql/12/main/postgresql.conf
        line: 'max_connections = 500'

    - name: Set listen addresses
      lineinfile:
        path: /etc/postgresql/12/main/postgresql.conf
        line: 'listen_addresses = "*"'
```
Which command would you use to run the configure_database.yml playbook in both check mode and diff mode?
A. ansible-playbook configure_database.yml --diff
B. ansible-playbook configure_database.yml --check
==C. ansible-playbook configure_database.yml --check --diff==
D. ansible-playbook --diff --check configure_database.yml
**A:** To run the configure_database.yml playbook in both check mode and diff mode, use ansible-playbook configure_database.yml --check --diff command.
#### 8. Q: Consider again the same sample playbook named configure_database.yml as shown below.
```js
- hosts: all
  tasks:
    - name: Set max connections
      lineinfile:
        path: /etc/postgresql/12/main/postgresql.conf
        line: 'max_connections = 500'

    - name: Set listen addresses
      lineinfile:
        path: /etc/postgresql/12/main/postgresql.conf
        line: 'listen_addresses = "*"'
```
To check the configure_database.yml playbook for syntax errors, which command would you use?
A. ansible-playbook configure_database.yml
B. ansible-playbook configure_database.yml --syntax
==C. ansible-playbook --syntax-check configure_database.yml==
D. ansible configure_database.yml --syntax-check
**A:** To check the configure_database.yml playbook for syntax errors, you can use **ansible-playbook --syntax-check configure_database.yml** command. (C)
#### 9. Q: You've been given a playbook named database_setup.yml that is supposed to set up a PostgreSQL database on your servers. Before deploying it, you want to ensure that it adheres to best practices and doesn't have any style-related issues.
```js
- name: Database Setup Playbook
  hosts: db_servers
  tasks:
    - name: Ensure PostgreSQL is installed
      apt:
        name: postgresql
        state: latest
        update_cache: yes

    - name: Start PostgreSQL service
      service:
        name: postgresql
        state: started

    - copy:
        src: /path/to/pg_hba.conf
        dest: /etc/postgresql/12/main/pg_hba.conf
      notify:
        - Restart PostgreSQL
```
Which command would you use to run ansible-lint on the database_setup.yml playbook?
A. ansible database_setup.yml --lint
==B. ansible-lint database_setup.yml==
C. ansible-playbook database_setup.yml --lint
D. lint-ansible database_setup.yml
**A:** To run ansible-lint on the database_setup.yml playbook, use **ansible-lint** **database_setup.yml** command. (B)
#### 10. Q: Consider again the same sample playbook named database_setup.yml as shown below.
```js
- name: Database Setup Playbook
  hosts: db_servers
  tasks:
    - name: Ensure PostgreSQL is installed
      apt:
        name: postgresql
        state: latest
        update_cache: yes

    - name: Start PostgreSQL service
      service:
        name: postgresql
        state: started

    - copy:
        src: /path/to/pg_hba.conf
        dest: /etc/postgresql/12/main/pg_hba.conf
      notify:
        - Restart PostgreSQL
```
After running ansible-lint on the playbook, which of the following issues might you expect to see?
==A. Incorrect indentation.==
B. Deprecated 'apt' module.
==C. Missing 'name' attribute for a task.==
D. Use of a blacklisted command.
**A:** After running ansible-lint on the playbook, the following issues might be seen.
A. Incorrect indentation. | C. Missing name attribute for a task.
#### 11. Q: You've been given feedback from ansible-lint about potential issues in your hypothetical webserver_setup.yml playbook. The feedback mentions issues with indentation, deprecated modules, and missing name attributes.

Which of the following is NOT a recommended action based on the feedback?
A. Correcting the indentation in the playbook.
B. Replacing deprecated modules with their newer counterparts.
==C. Ignoring the feedback and proceeding with playbook execution.==
D. Adding 'name' attributes to tasks that are missing them.

**A:** Ignoring the feedback and proceeding with playbook execution is definitely not a recommended action. (C)
#### 12. Q: If ansible-lint provides no output after checking a playbook, what does it indicate?
A. The playbook has syntax errors.
B. The playbook is empty.
==C. The playbook adheres to best practices and has no style-related issues.==
D. ansible-lint failed to check the playbook.
**A:** If Ansible-lint provides no output after checking a playbook, it indicates that, the playbook adheres to best practices and has no style-related issues. (C)
#### 13. Q: Update the name of the play in /home/bob/playbooks/playbook.yaml playbook to Execute a date command on localhost.
**A:** **Edit the playbook.**
`vi /home/bob/playbooks/playbook.yaml`

**Update the playbook as below:**
```js
- name: 'Execute a date command on localhost'
  hosts: localhost
  become: yes
  tasks:
     - name: 'Execute a date command'
       command: date
```
**Run the playbook.**
```
cd /home/bob/playbooks
ansible-playbook -i inventory playbook.yaml
```

> [!tip] terminal
> [bob@student-node playbooks]$ ansible-playbook -i inventory playbook.yaml
PLAY [Execute a date command on localhost] * * * * * * * * * * * * * * * * * * * * * *  
TASK [Gathering Facts] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
ok: [localhost]
TASK [Execute a date command] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *  
changed: [localhost]
PLAY RECAP * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *  
localhost                  : ==ok=2==    ==changed=1==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 

#### 14. Q: Update the playbook /home/bob/playbooks/playbook.yaml to add a task name Task to display hosts file for the existing task.
**A:** Edit the playbook.
`vi /home/bob/playbooks/playbook.yaml`

**Update the playbook as below:**
```js
- name: 'Execute a command to display hosts file on localhost'
  hosts: localhost
  become: yes
  tasks:
     - name: 'Task to display hosts file'
       command: 'cat /etc/hosts'
```

**Run the playbook.**
```
cd /home/bob/playbooks
ansible-playbook -i inventory playbook.yaml
```

> [!tip] terminal
> [bob@student-node playbooks]$ ansible-playbook -i inventory playbook.yaml
PLAY [Execute a command to display hosts file on localhost] * * * * * * * * * * * * *
TASK [Gathering Facts] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
ok: [localhost]
TASK [Task to display hosts file] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
changed: [localhost]
PLAY RECAP * * * * * * * * * * * * * * * * ** * * * * * * * * * * * * * * * * * * * * * * * * * 
localhost                  : ==ok=2==    ==changed=1==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

#### 15. Q: We have reset the playbook /home/bob/playbooks/playbook.yaml, now update it to add another task. The new task must execute the command cat /etc/resolv.conf and set its name to Task to display nameservers.
**A:** Edit the playbook.
`vi /home/bob/playbooks/playbook.yaml`
**Update the playbook as below.**

```js
- name: 'Execute two commands on localhost'
  hosts: localhost
  become: yes
  tasks:
     - name: 'Execute a date command'
       command: date
     - name: 'Task to display nameservers'
       command: 'cat /etc/resolv.conf'
```
**Run the playbook.**
```
cd /home/bob/playbooks
ansible-playbook -i inventory playbook.yaml
```

> [!tip] terminal
> [bob@student-node playbooks]$ ansible-playbook -i inventory playbook.yaml
PLAY [Execute two commands on localhost] * * * * * * * * * * * * * * * * * * * * * * *  
TASK [Gathering Facts] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
ok: [localhost]
TASK [Execute a date command] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *  
changed: [localhost]
TASK [Task to display nameservers] * * * * * * * * * * * * * * * * * * * * * * * * * * * *  
changed: [localhost]
PLAY RECAP * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *  
localhost                  : ==ok=3==    ==changed=2==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

#### 16. Q: So far, we have been running all tasks on localhost. We would now like to run these tasks on node01, this host is already defined in /home/bob/playbooks/inventory file. Update the playbook /home/bob/playbooks/playbook.yaml to run the tasks on the node01 host.
**A:** Edit the playbook.
`vi /home/bob/playbooks/playbook.yaml`
**Update the playbook as below.**
```js
- name: 'Execute two commands on node01'
  hosts: node01
  tasks:
     - name: 'Execute a date command'
       command: date
     - name: 'Execute a command to display hosts file'
       command: 'cat /etc/hosts'
```
**Run the playbook.**
```
cd /home/bob/playbooks
ansible-playbook -i inventory playbook.yaml
```

> [!tip] Terminal
> [bob@student-node playbooks]$ ansible-playbook -i inventory playbook.yaml
PLAY [Execute two commands on node01] * * * * * * * * * * * * * * * * * * * * * * * * 
TASK [Gathering Facts] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
ok: [node01]
TASK [Execute a date command] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
changed: [node01]
TASK [Task to display hosts file] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
changed: [node01]
PLAY RECAP * * * * * * * * * * * * * * * * * * * * * * * * ** * * * * * * * * * * * * * * * * * 
node01                     : ==ok=3==    ==changed=2==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 

#### 17. Q: Refer to the /home/bob/playbooks/inventory file. We would like to run the /home/bob/playbooks/playbook.yaml on all servers defined under web_nodes group.

Note: Use the group name in playbook as defined in the inventory file.
**A:** Edit the playbook.
`vi /home/bob/playbooks/playbook.yaml`
**Update the playbook as below.**
```js
- name: 'Execute two commands on web_node1'
  hosts: web_nodes
  tasks:
     - name: 'Execute a date command'
       command: date
     - name: 'Execute a command to display hosts file'
       command: 'cat /etc/hosts'
```
**Run the playbook.**
```
cd /home/bob/playbooks
ansible-playbook -i inventory playbook.yaml
```

> [!tip] Terminal
> [bob@student-node playbooks]$ ansible-playbook -i inventory playbook.yaml
PLAY [Execute two commands on web_nodes]* * * * * * * * * * * * * * * * * * * * * 
TASK [Gathering Facts]* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
ok: [node01]
ok: [node02]
TASK [Execute a date command] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *   
changed: [node01]
changed: [node02]
TASK [Task to display hosts file] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *   
changed: [node01]
changed: [node02]
PLAY RECAP * * * * * * * * * * * * * * * * * * * * * * * * * * ** * * * * * * * * * * * * * * * 
node01                     : ==ok=3==    ==changed=2==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
node02                     : ==ok=3==    ==changed=2==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 

#### 18. Q: Update the /home/bob/playbooks/playbook.yaml to add a new play named Execute a command on node02, and a task under it to execute cat /etc/hosts command on node02 host, name the task Task to display hosts file on node02.
Refer to the given inventory file.
**A:** Edit the playbook.
`vi /home/bob/playbooks/playbook.yaml`
**Update the playbook as below.**
```js
- name: 'Execute two commands on node01'
  hosts: node01
  become: yes
  tasks:
    - name: 'Execute a date command'
      command: date
    - name: 'Task to display hosts file on node01'
      command: 'cat /etc/hosts'
- name: 'Execute a command on node02'
  hosts: node02
  become: yes
  tasks:
     - name: 'Task to display hosts file on node02'
       command: 'cat /etc/hosts'
```
**Run the playbook.**
```
cd /home/bob/playbooks
ansible-playbook -i inventory playbook.yaml
```

> [!tip] Terminal
> [bob@student-node playbooks]$ ansible-playbook -i inventory playbook.yaml
PLAY [Execute two commands on node01] * * * * * * * * * * * * * * * * * * * * * * * *  
TASK [Gathering Facts] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *  
ok: [node01]
TASK [Execute a date command] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *  
changed: [node01]
TASK [Task to display hosts file on node01] * * * * * * * * * * * * * * * * * * * * * * * *  
changed: [node01]
PLAY [Execute a command on node02] * * * * * * * * * * * * * * * * * * * * * * * * * *  
TASK [Gathering Facts] * * * * * * * * * * ** * * * * * * * * * * * * * * * * * * * * * * * *  
ok: [node02]
TASK [Task to display hosts file on node02] * * * * * * * * * * * * * * * * * * * * * * * * 
changed: [node02]
PLAY RECAP * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *  
node01                     : ==ok=3==    ==changed=2==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
node02                     : ==ok=2==    ==changed=1==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

### LAB - MODULES
#### 1. Q:Which of the following Ansible modules support free_form parameter?
**A:** Ansible **command** module support free_form parameter.
#### 2. Q: Has Ansible support idempotancy.
**A:** yes
#### 3. Q: Which of the following commands we can use to see the information about Ansible modules from command line?
**A:** Ansible-doc
#### 4. Q: Which Ansible module is used in the following playbook?
```js
- hosts: localhost
  become: yes
  become_user: root
  tasks:
    - name: create user
      user:
        name: admin
```
**A:** user
#### 5. Q: Which of the following statements are true about lineinfile Ansible module?
A. It only adds the given line in file if that line doesn't exist in that file.
B. It adds the given line in file even if that line already exists in that file.
C. It replaces all existing lines in the file with a new given line.
D. It keeps the existing lines as well and add a new given line in the file.
**A:** A. & D. 
#### 6. Q: What are Ansible system modules used for?
A. System modules are basically used to create/update files and directories.
==B. System modules are actions to be performed at a system level such as modifying the users and groups on a system, modifying iptables, starting/stopping the service etc.==
C. System modules are used to execute commands or scripts on a system.
D. System modules are used to install and setup packages on a system.
**A:** B
#### 7. Q: Your organization uses a proprietary cloud service that is not natively supported by Ansible. You need to develop a custom Ansible module to interact with this cloud service's API and provision resources.
Which type of the Ansible plugins allows you to integrate with a cloud provider's API for custom resource provisioning?
a. Connection Plugin
b. Lookup Plugin
==c. Module Plugin==
d. Action Plugin
**A:** Module Plugin allows you to integrate with a cloud provider's API for custom resource provisioning.
#### 8. Q: You've developed a custom module named custom_cloud. To test this module in a playbook named deploy.yml, which of the following task definitions is correct?
**a.**
```
- name: Provision custom cloud resource
  action: custom_cloud
  args:
    param1: value1
    param2: value2
```
**b.**
```
- name: Provision custom cloud resource
  module: custom_cloud
  parameters:
    - param1: value1
    - param2: value2
```
==**c.**==
```
- name: Provision custom cloud resource
  custom_cloud:
    param1: value1
    param2: value2
```
**d.**
```
- name: Provision custom cloud resource
  use_module: custom_cloud
  with_args:
    param1: value1
    param2: value2
```
**A:** C
#### 9. Q: You are tasked with setting up an Ansible playbook that automates the deployment of applications on AWS ec2 instances. Before running the playbook, you need to ensure that Ansible has an up-to-date inventory of all ec2 instances in your AWS account.
Which type of Ansible plugin would you use to fetch real-time information about your AWS ec2 instances?
a. Lookup Plugin
b. Filter Plugin
c. Callback Plugin
==d. Dynamic Inventory Plugin==
**A:** Dynamic Inventory Plugin will be used to fetch real-time information about your AWS ec2 instances.
#### 10. Q: You have a custom dynamic inventory script named aws_inventory.py. Which command would you use to list all hosts in your AWS inventory using this script?
==a. ansible-inventory --list -i aws_inventory.py==
b. ansible-playbook --inventory aws_inventory.py
c. ansible aws_inventory.py --list-hosts
d. ansible-list --inventory aws_inventory.py
**A:**  A

> [!success]
> **ansible-inventory** is the tool
> The command needs to know which inventory to inspect
> **-i aws_inventory.py** tells it exactly which inventory source (your script) to use
> Even though the command name suggests inventory, you can have multiple inventories or scripts, so **-i** clarifies which one to use for this run.
> 
> It's similar to opening a file: the command is "open," but you still need to specify which file.

#### 11. Q: You are tasked with finding a collection in Ansible that provides modules specifically designed for managing Cisco IOS devices. Using the Modules & Plugins Index, which of the following collections is best suited for this purpose?
a. cisco.config
b. cisco.ios
c. cisco.setup
d. cisco.network
**A:** B
#### 12. Q: Which of the following is not a common connection parameter used by modules within the cisco.ios collection for managing Cisco IOS devices?
a. hostname
b. password
==c. ios_version==
d. username
**A:**
#### 13. Q: Which of the following Ansible versions is cisco.ios module likely to be compatible with?
Note: This can be a hypothetical answer; the actual version compatibility would need to be checked in the Modules & Plugins Index.
a. Ansible 1.5
==b. Ansible 2.8==
c. Ansible 3.1
d. Ansible 4.0
**A:**
#### 14. Q: You're planning to deploy an application on AWS and need to set up ec2 instances using Ansible. Which module is primarily used for managing ec2 instances?
a. aws_instance
b. aws_ec2_config
==c. ec2_instance==
d. aws_setup
**A:**
#### 15. Q: Update the playbook named playbook.yaml under /home/bob/playbooks directory with a task named Execute a script to run a script. The script is located at /tmp/install_script.sh on student-node.
Use the script module.
Note: There is already an inventory file /home/bob/playbooks/inventory present on student-node system.
**A:** Edit the playbook 
```
/home/bob/playbooks/playbook.yaml
vi /home/bob/playbooks/playbook.yaml 
```
**Update the playbook as below.**
```js
- name: 'hosts'
  hosts: all
  become: yes
  tasks:
    - name: 'Execute a script'
      script: '/tmp/install_script.sh'
```
**Run the playbook.**
```
cd /home/bob/playbooks/
ansible-playbook -i inventory playbook.yaml 
```
> **ansible-playbook:** This command runs an Ansible playbook, which is a YAML file defining tasks to automate on multiple hosts.
> **-i inventory:** The -i option specifies the inventory file, which lists the target hosts or groups. Here, inventory is the filename.
> playbook.yaml: The playbook file containing the automation instructions.
> **Why -i inventory?**
> Because Ansible needs to know which hosts to target. The inventory file provides that info.
> 
> **Why not just -i playbook.yaml?**
> Because **playbook.yaml** is the playbook, not the inventory. The **-i** flag always points to the inventory file, not the playbook.
> 
> **In summary:**
> **ansible-playbook** runs the automation.
> **-i inventory** tells it which hosts to target.
> **playbook.yaml** is the set of tasks to execute.


> [!tip] Terminal
> PLAY [hosts] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
TASK [Gathering Facts] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
ok: [node02]
ok: [node01]
TASK [Execute a script] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *  
changed: [node02]
changed: [node01]
PLAY RECAP * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
node01                     : ==ok=2==    ==changed=1==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
node02                     : ==ok=2==    ==changed=1==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
#### 16. Q: Update the playbook /home/bob/playbooks/playbook.yaml to add a new task to start httpd service on all web nodes defined in /home/bob/playbooks/inventory file.
Use the service module.
**A:** Edit playbook 
```
/home/bob/playbooks/playbook.yaml:
vi /home/bob/playbooks/playbook.yaml 
```
**Update the playbook as below.**
```js
- name: 'hosts'
  hosts: all
  become: yes
  tasks:
    - name: 'Execute a script'
      script: '/tmp/install_script.sh'
    - name: 'Start httpd service'
      service:
        name: 'httpd'
        state: 'started'
```
**Run the playbook.**
```
cd /home/bob/playbooks/
ansible-playbook -i inventory playbook.yaml 
```

> [!tip]- Terminal
>PLAY [hosts] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
TASK [Gathering Facts] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *  
ok: [node02]
ok: [node01]
TASK [Execute a script] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
changed: [node02]
changed: [node01]
TASK [Start httpd service] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
changed: [node02]
changed: [node01]
PLAY RECAP * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
node01                     : ==ok=3==    ==changed=2==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
node02                     : ==ok=3==    ==changed=2==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 

> [!info]
> > In Ansible, you need to specify the module for each task unless you're using a shortcut like raw: or include:.
> > 
> > How to know if it's a module?
> > 
> > Modules are specific tasks like **service**:, **command**:, **file**:, **yum**:, etc.
> > They are documented in the Ansible Module Index.
> > **Common modules include:**
> > - **service** (manage services)
> > - **command** (run commands)
> > - **shell** (run shell commands)
> > - **file** (manage files/directories)
> > - **yum** / **apt** (package managers)
> > - **copy** (copy files)
> > - **template** (render templates)
> > - **user** (manage users)
> > - To list available modules:
> > **Run:**
> > `ansible-doc -l
> > This command lists all modules installed and available for use in your environment!

#### 17. Q: Update the playbook /home/bob/playbooks/playbook.yaml to append the /var/www/html/index.html file on all web nodes. The line needs to be added is Welcome to ansible-beginning course, create the index.html file if doesn't exist.
Use the lineinfile module
**A:** Edit file playbook:
`vi /home/bob/playbooks/playbook.yaml `
**Update playbook as below:**
``` js
- name: 'hosts'
  hosts: all
  become: yes
  tasks:
    - name: 'Execute a script'
      script: '/tmp/install_script.sh'
    - name: 'Start httpd service'
      service:
        name: 'httpd'
        state: 'started'
    - name: "Create or update index.html file."
      lineinfile:
        path: /var/www/html/index.html
        line: "Welcome to ansible-beginning course"
        create: true

```
**Run the playbook.**
```
cd /home/bob/playbooks/
ansible-playbook -i inventory playbook.yaml 
```
> [!info]
> Great question! Both single ' and double " quotes are valid in YAML and Ansible task names. The choice depends on your needs:
> 
> **Single quotes '**: Treat the content literally; no variable interpolation or escape sequences.
> **Double quotes "**: Allow variable interpolation and escape sequences within the string.
> In your case, since the task name is plain text without variables or special characters, both work equally well. Use single quotes for simplicity, or double quotes if you plan to include variables or special characters later. 😊
#### 18. Q: Update the playbook /home/bob/playbooks/playbook.yaml to add a new task to create a new user called web_user.
Use the user module for this task. You can find the user details as below.
Username: web_user
uid: 1040
group: developers
**A:** Edit the playbook:
`vi /home/bob/playbooks/playbook.yaml `
**Update the playbook as below:**
```js
- name: 'hosts'
  hosts: all
  become: yes
  tasks:
    - name: 'Execute a script'
      script: '/tmp/install_script.sh'
    - name: 'Start httpd service'
      service:
        name: 'httpd'
        state: 'started'
    - name: "Update /var/www/html/index.html"
      lineinfile:
        path: /var/www/html/index.html
        line: "Welcome to ansible-beginning course"
        create: true
    - name: 'Create a new user'
      user:
        name: 'web_user'
        uid: 1040
        group: 'developers'
```
**Run the playbook.**
```
cd /home/bob/playbooks/
ansible-playbook -i inventory playbook.yaml 
```

> [!tip]- Terminal
> PLAY [hosts] ** * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
TASK [Gathering Facts] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
ok: [node02]
ok: [node01]
TASK [Execute a script] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
changed: [node01]
changed: [node02]
TASK [Start httpd service] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *  
ok: [node01]
ok: [node02]
TASK [Update /var/www/html/index.html] * * * * * * * * * * * * * * * * * * * * * * * * *  
ok: [node02]
ok: [node01]
TASK [Create a new web user]* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
changed: [node02]
changed: [node01]
PLAY RECAP * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *  
node01                     : ==ok=5==    ==changed=2==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
node02                     : ==ok=5==    ==changed=2==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

#### 19. Q: Differences between INVENTORY and PLAYBOOK
**A:** Inventory file: Defines the hosts and groups of hosts. Usually a plain text file listing IPs or hostnames, or in INI/YAML format. Example:
```
  [web]
  node01
  node02
```

Playbook file: YAML file that contains tasks, roles, and configurations to run on hosts. Example:
```js
  - hosts: web
    tasks:
      - name: start httpd
        service:
          name: httpd
          state: started
```

> [!tip]- Terminal
> PLAY [**hosts**]* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
TASK [Gathering Facts]* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *  
ok: [node02]
ok: [node01]
TASK [Execute a script] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *  
changed: [node01]
changed: [node02]
TASK [Start httpd service] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
ok: [node02]
ok: [node01]
TASK [Create or update index.html file.] * * * * * * * * * * * * * * * * * * * * * * * * *   
changed: [node01]
changed: [node02]
PLAY RECAP * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
node01                     : ==ok=4==    ==changed=2==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
node02                     : ==ok=4==    ==changed=2==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

> [!todo] Note
> Tip: **Inventory** files are used to specify **where to run**, and **playbooks** specify **what to do!**
> > Both can be .yaml, but it's common to see:
> > Inventory files: Often named **inventory**, **hosts**, or similar, and can be **.ini**, **.yaml**, or **.json**.
> > Playbook files: Usually named **playbook.yaml**, **site.yaml**, or similar, and are definitely **.yaml**.
> > So, yes, both can be **.yaml**, but their roles are different. 
> > Just remember: ***inventory files define hosts, playbooks define tasks!***


### ANSIBLE VARIABLES
You teach best what you most need to learn
#### 1. Q: Can we define variables in an Ansible inventory file?
**A:** yes
#### 2. Q: Which of the following formats is used to call a variable in an Ansible playbook?
**A:** '{{ variable_name }}'
#### 3. Q: The playbook located at /home/bob/playbooks/playbook.yaml is designed to add a name server entry in the sample file /tmp/resolv.conf on localhost. The name server information is already defined as a variable named nameserver_ip within the /home/bob/playbooks/inventory file.
Your task is to replace the hardcoded IP address of the name server in the playbook with the value from the nameserver_ip variable specified in the inventory file.
Note: You need not to execute this playbook as of now.
``` js
[bob@student-node playbooks]$ cat playbook.yaml 
---
- name: 'Add nameserver in resolv.conf file on localhost'
  hosts: localhost
  become: yes
  tasks:
    - name: 'Add nameserver in resolv.conf file'
      lineinfile:
        path: /tmp/resolv.conf
        line: 'nameserver 8.8.8.8'

[bob@student-node playbooks]$ cat inventory 
localhost ansible_connection=local nameserver_ip=8.8.8.8 snmp_port=160-161
node01 ansible_host=node01 ansible_ssh_pass=caleston123
node02 ansible_host=node02 ansible_ssh_pass=caleston123
[web_nodes]
node01
node02

[all:vars]
app_list=['vim', 'sqlite', 'jq']
user_details={'username': 'admin', 'password': 'secret_pass', 'email': 'admin@example.com'}
```
**A:** Edit the playbook.
vi /home/bob/playbooks/playbook.yaml
**Update the playbook as below.**
```js
- name: 'Add nameserver in resolv.conf file on localhost'
  hosts: localhost
  become: yes
  tasks:
    - name: 'Add nameserver in resolv.conf file'
      lineinfile:
        path: /tmp/resolv.conf
        line: 'nameserver {{  nameserver_ip  }}'
```
#### 4. Q: We have updated the /home/bob/playbooks/playbook.yaml playbook to include a new task for disabling the SNMP port on localhost. However, the port number is currently hardcoded. Please update the playbook to replace the hardcoded value of the SNMP port with the value from the variable named snmp_port, which is defined in the inventory file.
Note: You need not to execute this playbook as of now.
**A:** Edit the playbook:
`vi /home/bob/playbooks/playbook.yaml`
**Update the playbook as below:**
```js
- name: 'Add nameserver in resolv.conf file on localhost'
  hosts: localhost
  become: yes
  tasks:
    - name: 'Add nameserver in resolv.conf file'
      lineinfile:
        path: /tmp/resolv.conf
        line: 'nameserver {{  nameserver_ip  }}'
    - name: 'Disable SNMP Port'
      firewalld:
        port: '{{ snmp_port }}'
        permanent: true
        state: disabled
```
#### 5. Q: We have reset the /home/bob/playbooks/playbook.yaml playbook. It is currently printing some personal information of an employee.
Move the car_model, country_name, and title values to variables defined at the play level.
Add three new variables named car_model, country_name, and title under the play and use these variables within the tasks to remove the hardcoded values.
**A:** Edit the playbook:
`vi /home/bob/playbooks/playbook.yaml`
**Update the playbook as below:**
```js
- hosts: localhost
  vars:
    car_model: 'BMW M3'
    country_name: USA
    title: 'Systems Engineer'
  tasks:
    - command: 'echo "My car is {{ car_model }}"'
    - command: 'echo "I live in the {{ country_name }}"'
    - command: 'echo "I work as a {{ title }}"'
```
**Run the playbook:**
``` shell
cd /home/bob/playbooks
ansible-playbook -i inventory playbook.yaml
```

> [!failure]- Terminal
> [bob@student-node playbooks]$ ansible-playbook -i inventory playbook.yam
PLAY [localhost]  * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
TASK [Gathering Facts]  * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
ok: [localhost]
TASK [command]  * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
fatal: [localhost]: FAILED! => {"msg": "The task includes an option with an undefined variable. The error was: 'car_model' is undefined. 'car_model' is undefined\n\nThe error appears to be in '/home/bob/playbooks/playbook.yaml': line 4, column 7, but may\nbe elsewhere in the file depending on the exact syntax problem.\n\nThe offending line appears to be:\n\n  tasks:\n    - command: 'echo \"My car is {{car_model}}\"'\n      ^ here\nWe could be wrong, but this one looks like it might be an issue with\nmissing quotes. Always quote template expression brackets when they\nstart a value. For instance:\n\n    with_items:\n      - {{ foo }}\n\nShould be written as:\n\n    with_items:\n      - \"{{ foo }}\"\n"}
PLAY RECAP  * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
localhost                  : ==ok=1==    ==changed=0==    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0   
#### 6. Q: The /home/bob/playbooks/app_install.yaml playbook is responsible for installing a list of packages on remote server(s). The list of packages to be installed is already added to the /home/bob/playbooks/inventory file as a list variable called app_list.
Right now the list of packages to be installed is hardcoded in the playbook. Update the /home/bob/playbooks/app_install.yaml playbook to replace the hardcoded list of packages to use the values from the app_list variable defined in the inventory file. Once updated, please run the playbook once to make sure it works fine.
**A:** Edit the playbook:
`vi /home/bob/playbooks/app_install.yaml`
**Update the playbook as below:**

```js
- hosts: web_nodes
  become: yes
  tasks:
    - name: Install applications
      yum:
        name: "{{ item }}"
        state: present
      with_items:
        - "{{ app_list }}"
```
**Run the playbook.**
```shell
cd /home/bob/playbooks
ansible-playbook -i inventory app_install.yaml
```

> [!failure]- Terminal 
> [bob@student-node playbooks]$ ansible-playbook -i inventory app_install.yaml
PLAY [web_nodes]* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
TASK [Gathering Facts] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
ok: [node01]
ok: [node02]
TASK [Install applications]* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
changed: [node01] => (item=vim)
changed: [node02] => (item=vim)
changed: [node01] => (item=sqlite)
changed: [node01] => (item=jq)
changed: [node02] => (item=sqlite)
changed: [node02] => (item=jq)
PLAY RECAP * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
node01                     : ==ok=2==    ==changed=1==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
node02                     : ==ok=2==    ==changed=1==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  


> [!tip]- Terminal - Correct
> [bob@student-node playbooks]$ ansible-playbook -i inventory **app_install.yaml**
PLAY [web_nodes] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
TASK [Gathering Facts] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
ok: [node01]
ok: [node02]
TASK [Install applications] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
ok: [node01] => (item=vim)
ok: [node02] => (item=vim)
ok: [node01] => (item=sqlite)
ok: [node02] => (item=sqlite)
ok: [node01] => (item=jq)
ok: [node02] => (item=jq)
PLAY RECAP * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
node01                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
node02                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

> [!warning] Double quotes
> quotes double in var
> name: "{{ item }}"

```java
[bob@student-node playbooks]$ cat inventory 
localhost ansible_connection=local nameserver_ip=8.8.8.8 snmp_port=160-161
node01 ansible_host=node01 ansible_ssh_pass=caleston123
node02 ansible_host=node02 ansible_ssh_pass=caleston123
[web_nodes]
node01
node02

[all:vars]
app_list=['vim', 'sqlite', 'jq']
user_details={'username': 'admin', 'password': 'secret_pass', 'email': 'admin@example.com'}
```

> [!note] Become
> The become keyword in Ansible is used to elevate privileges, typically to run commands as another user, often root. When you set become: yes, Ansible will use privilege escalation (like sudo) to execute tasks with higher permissions. It’s essential for tasks that require admin rights, such as installing packages or modifying system files.

#### 7. Q: The /home/bob/playbooks/user_setup.yaml playbook is responsible for setting up a new user on a remote server(s). The user details like username, password, and email are already added to the /home/bob/playbooks/inventory file as a dictionary variable called user_details.
Right now the user details is hardcoded in the playbook. Update the /home/bob/playbooks/user_setup.yaml playbook to replace the hardcoded values to use the values from the user_details variable defined in the inventory file. Once updated, please run the playbook once to make sure it works fine.
**A:** Edit the playbook:
`vi /home/bob/playbooks/user_setup.yaml`
**Update the playbook as below:**
```js
- hosts: all
  become: yes
  tasks:
    - name: Set up user
      user:
        name: "{{ user_details.username }}"
        password: "{{ user_details.password }}"
        comment: "{{ user_details.email }}"
        state: present
```
**Run the playbook:**
```
cd /home/bob/playbooks
ansible-playbook -i inventory user_setup.yaml
```

> [!tip]- Terminal
> [bob@student-node playbooks]$ ansible-playbook -i inventory user_setup.yaml
PLAY [all] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
TASK [Gathering Facts] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
ok: [localhost]
ok: [node01]
ok: [node02]
TASK [Set up user] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
[WARNING]: The input password appears not to have been hashed. The 'password' argument must be encrypted for this module to work
properly.
changed: [localhost]
changed: [node01]
changed: [node02]
PLAY RECAP * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
localhost                  : ==ok=2==    ==changed=1==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
node01                     : ==ok=2==    ==changed=1==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
node02                     : ==ok=2==    ==changed=1==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

> [!caution] Quotation use
> Use double quotes " " for variable interpolation.
Use single quotes ' ' for literal strings.
The word outside {{ }} is static text.

> [!note] Quotes variables
> **'nameserver {{ nameserver_ip }}'** (single quotes): The entire string is treated as plain text. The variable {{ nameserver_ip }} won't be expanded; it will appear exactly as written.
**"nameserver {{ nameserver_ip }}"** (double quotes): The string allows variable interpolation. The {{ nameserver_ip }} will be replaced with its actual value during execution.
Example:
If nameserver_ip=8.8.8.8:
name: Example with single quotes
  line: 'nameserver {{ nameserver_ip }}'# Output: nameserver {{ nameserver_ip }}
-name: Example with double quotes
  -line: "nameserver {{ nameserver_ip }}"# Output: nameserver 8.8.8.8
Use double quotes when you want variables to be expanded! 🚀

#### LAB- CONDITIONAL
#### 1. Q: Which of the following Ansible built-in variable populates the flavour of the operating system?
**A:** **ansible_os_family** is the Ansible built-in variable that populates the flavour of the operating system.

```java
TERMINAL
> [bob@student-node ~]$ ansible localhost -m setup -a 'filter=ansible_os_family'
   localhost | SUCCESS => {
	 "ansible_facts": {
        "ansible_os_family": "RedHat"
    },
    "changed": false
}
The -a option in the ansible command stands for arguments. 
The -m option in the ansible command specifies the module you want to run.
```
#### 2. Q: Which keyword is used to define a condition in an Ansible playbook?
**A:** when keyword is used to define a condition
#### 3. Q: As per the given playbook, will Ansible install the vim package on a RedHat based machine?
```js
- name: Install package
  hosts: app1
  tasks:
    - name: Install
      package:
        name: vim
        state: present
      when: ansible_os_family != "RedHat"
```
**A:** No

#### 4. Q: There is a playbook named nginx.yaml under /home/bob/playbooks directory. It is starting nginx service on all hosts defined in /home/bob/playbooks/inventory inventory file. Use the when condition to run this task only on node02 host.
**A:** Edit 'nginx.yaml' playbook.
`vi /home/bob/playbooks/nginx.yaml`
**Updated the playbook as below.**

```js
-  name: 'Execute a script on all web server nodes'
   hosts: all
   become: yes
   tasks:
     -  service: 'name=nginx state=started'
        when: 'ansible_host=="node02"'
```
**Run the playbook:**
cd /home/bob/playbooks
ansible-playbook -i inventory nginx.yaml
> [!tip] Terminal
> [bob@student-node playbooks]$ ansible-playbook -i inventory nginx.yaml 
PLAY [Execute a script on all web server nodes] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
TASK [Gathering Facts] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
ok: [node02]
ok: [node01]
TASK [service] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
skipping: [node01]
ok: [node02]
PLAY RECAP * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
node01                     : ==ok=1==    ==changed=0==    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0   
node02                     : ==ok=2==    ==changed=0==    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
#### 5. Q: The playbook under /home/bob/playbooks/age.yaml , has a variable defined called age. The two tasks attempt to print if I am a child or an Adult. Use the when conditional to print if I am a child or an Adult based on whether my age is *< 18 (Child)* or *>= 18 (Adult)*.
**A:** Edit playbook 'age.yaml'
`vi /home/bob/playbooks/age.yaml`
**Update the playbook as below.**

```js
- name: 'Am I an Adult or a Child?'
  hosts: localhost
  vars:
    age: 25
  tasks:
    - name: I am a Child
      command: 'echo "I am a Child"'
      when: 'age < 18'
    - name: I am an Adult
      command: 'echo "I am an Adult"'
      when: 'age >= 18'
```
**Run the playbook:**
```
cd /home/bob/playbooks
ansible-playbook -i inventory age.yaml
```

> [!tip] Terminal
> [bob@student-node playbooks]$ ansible-playbook -i inventory age.yaml 
PLAY [Am I an Adult or a Child?] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * TASK [Gathering Facts] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
ok: [localhost]
TASK [I am a Child] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
skipping: [localhost]
TASK [I am an Adult] * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
changed: [localhost]
PLAY RECAP * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
localhost                  : ==ok=2==    ==changed=1==    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0 
#### 6. Q: Playbook /home/bob/playbooks/nameserver.yaml attempts to add an entry in /etc/resolv.conf file to add a new nameserver.

The first task in the playbook is using the shell module to display the existing contents of **/etc/resolv.conf** file and the second one is adding a new line containing the name server details into the file. However, when this playbook is run multiple times, it keeps adding new entries of same line into the resolv.conf file. To resolve this issue, update the playbook as per details mentioned below.

1. Add a register directive to store the output of the first task to a variable called command_output
2. Then add a conditional to the second task to check if the output already contains the name server (10.0.250.10). Use command_output.stdout.find(\<IP>) == -1
**Note:**
**a.**  A better way to do this would be to use the lineinfile module. This is just for practice.
**b.** shell and command modules are similar in a way that they are used to execute a command on the system. However, shell executes the command inside a shell giving us access to environment variables and redirection using >>.
**A:** Edit playbook 'nameserver.yaml'
vi /home/bob/playbooks/nameserver.yaml
**Update the playbook as below.**
```js
- name: 'Add name server entry if not already entered'
  hosts: localhost
  become: yes
  tasks:
    - shell: 'cat /etc/resolv.conf'
      register: command_output
    - shell: 'echo "nameserver 10.0.250.10" >> /etc/resolv.conf'
      when: 'command_output.stdout.find("10.0.250.10") == -1'
```
**Run the playbook:**
```
cd /home/bob/playbooks
ansible-playbook -i inventory nameserver.yaml
```

> [!warn] Remember
> 
> The secret of success is to do the common things uncommonly well.
### LAB - LOOPS
#### 1. Q: Can loops be executed on dictionary values in Ansible?
**A:** Yes
#### 2. Q: Which type of plugin with_* directives use in Ansible?
**A:** Lookup
#### 3. Q: The playbook /home/bob/playbooks/fruits.yml currently runs an echo command to print a fruit name. Apply a loop directive (with_items) to the task to print all fruits defined under the fruits variable.
**A:** Update the contents of /home/bob/playbooks/fruits.yml playbook as below:

```js
-  name: 'Print list of fruits'
   hosts: localhost
   vars:
     fruits:
       - Apple
       - Banana
       - Grapes
       - Orange
   tasks:
     - command: 'echo "{{ item }}"'
       with_items: '{{ fruits }}'
```
**You can test the playbook:**

cd /home/bob/playbooks/
ansible-playbook -i localhost fruits.yml

> [!tip] Terminal
> [bob@student-node playbooks]$ ansible-playbook -i inventory fruits.yml 

PLAY [Print list of fruits] *****************************************************
TASK [Gathering Facts] ******************************************************
ok: [localhost]
TASK [command] ************************************************************
changed: [localhost] => (item=Apple)
changed: [localhost] => (item=Banana)
changed: [localhost] => (item=Grapes)
changed: [localhost] => (item=Orange)
PLAY RECAP ************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  


#### 4. Q: We are attempting to install multiple packages using the yum module for a more realistic use case. The playbook /home/bob/playbooks/packages.yml installs only a single package. Update it to install all packages defined under packages variable.
**A:** Update the contents of /home/bob/playbooks/packages.yml playbook as below:

- name: 'Install required packages'
  hosts: localhost
  become: yes
  vars:
    packages:
      - httpd
      - make
      - vim
  tasks:
    - yum:                                      ///siblings
        name: '{{ item }}'
        state: present
      with_items: '{{ packages }}' /// siblings


> [!tip] Terminal
> [bob@student-node playbooks]$ ansible-playbook -i inventory packages.yml 

PLAY [Install required packages] ************************************************************
TASK [Gathering Facts] **********************************************************************
ok: [localhost]
TASK [yum] **********************************************************************************
ok: [localhost] => (item=httpd)
ok: [localhost] => (item=make)
changed: [localhost] => (item=vim)
PLAY RECAP **********************************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  

ROLES
#### 1. Find a role on Ansible Galaxy
**A:** ansible-galaxy search mysql
#### 1. Use a role 
**A:** ansible-galaxy install  geerlingguy.mysql