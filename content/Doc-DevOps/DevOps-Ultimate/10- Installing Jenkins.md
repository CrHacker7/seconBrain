#### LAB
***Now let us install Jenkins on the centos-host machine and configure it to run on port 8090 instead of the default port 8080.*
Execute below given commands one by one:
`sudo yum install epel-release -y`
`sudo yum install fontconfig java-17-openjdk -y`
`sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo --no-check-certificate`
`sudo rpm --import http://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key`
`sudo yum install jenkins -y`
*Edit /lib/systemd/system/jenkins.service file and change Jenkins port to 8090 by updating Environment="JENKINS_PORT=" variable value.*
`sudo vi /lib/systemd/system/jenkins.service`
**It should look like this:***
Environment="JENKINS_PORT=8090"
***Once done start Jenkins service.***
`sudo systemctl start jenkins`

***Find the password to create the admin user***
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

***Complete its installation from UI and make sure to create first admin user as per details mentioned below:***
Username: admin
Password: Admin321
Full name: Admin User
E-mail address: admin@example.com
Under Customize Jenkins, select install ***suggested plugins*** option.
***Follow the on-screen instructions as given in the hint and further as below:***
1. Under Create First Admin User, enter the required details as given in the question.
2. Then click on Save and Continue.
3. Under Instance Configuration, you can click on Not now.
4. Finally click on Start using Jenkins button.

## Jenkins CLI
#### LAB
**How many users are currently configured for this Jenkins instance?**
Manage jenkins -> Users
- UserName: admin
- Password: Adm!n321

***Switch to this user by running the below command:***
root@jenkins-server:~# `id mike`
uid=1001(mike) gid=1002(mike) groups=1002(mike),27(sudo)
root@jenkins-server:~# `su - mike` 
mike@jenkins-server:~$

***There is a SSH key-pair created under /home/mike/.ssh.
What is name of the public key?***
The public key has a .pub extension and is called jenkins_key.pub
```json
mike@jenkins-server:~$ ls -lah
total 24K
drwxr-xr-x 3 mike mike 4.0K Jan 10 11:47 .
drwxr-xr-x 1 root root 4.0K Jan 10 11:47 ..
-rw-r--r-- 1 mike mike  220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 mike mike 3.7K Feb 25  2020 .bashrc
-rw-r--r-- 1 mike mike  807 Feb 25  2020 .profile
drwx------ 2 mike mike 4.0K Jan 10 11:47 .ssh
mike@jenkins-server:~/.ssh$ ls
jenkins_key  jenkins_key.pub
```
***Now add this public key (/home/mike/.ssh/jenkins_key.pub) in Jenkins for the user called mike. This will allow us to interact with Jenkins using the CLI.***
```json
mike@jenkins-server:~/.ssh$ cat jenkins_key.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC1yJiu3CiltL5n/An7q5lviXtJ8zJuAVZmMOtvE4TFZ/7Cnb+M3b3SVikwhHofvIBVWohGDnHbrHic3phjgYMI2d9zNonP+Qz7AJLplluEKg8XkHvN/KH/LjLfwldNkSLoUWD4dcp7V7ydh34/zpr9F1TJH+S3CWRw8Nlj9wLuvejdnBQ1MjaHDTxw19AJsG9Pm1mpn4XA5yl82UdfSugdcqakyZkjjULLOrjcXhoHoA5xyIF7pyltSE7iC9zkg0WjYU2bwfashaqCyYNLz/eEDc9zk+G41m/alJaBd7SsAIIVsRRgMoo+skl2B6RSTEAwqYdNrjZEmJlydlojG16LG+GS+he70b7dbmAR1Fr5JTOjjFoq4zgBkCoRe0qtV6rgAOs4lmPYJVAiMlX0Xdn7BuuTb3oLSBBS067J1yQprIYQFE1WO1cOvrZSquOqyBySdsqfHlQ5wYVP5j4fjMqWPcse8p0DrRSEL3VnpfZsidSZSx1Cz933Wi9LVeWbmF0= mike@jenkins-server
//copiar desde ssh-rsa... hasta el final "...server"
```
Next, add this key for the user (Navigate to People --> mike --> Configure --> SSH Public Keys)
Paste the key and hit Save.

***We have configured the SSH service in Jenkins to listen on a fixed port. To find out the port in use, run the command below:***
`curl -Lv http://localhost:8085/login 2>&1 | grep -i 'x-ssh-endpoint'`
```js
mike@jenkins-server:~/.ssh$ curl -Lv http://localhost:8085/login 2>&1 | grep -i 'x-ssh-endpoint'
< X-SSH-Endpoint: 8085-port-d22abvevjpqhdb7d.labs.kodekloud.com:8022
//ANSWER 8022
```
***Which port does the Jenkins SSH service use?***
You can also check the port used by navigating to Dashboard ---> Manage Jenkins --> Security --> SSH Server
***Now that we have the port used by the Jenkins SSH server, let us begin interacting with Jenkins over ssh with the user mike.
As mike, try running the following commands:***
mike@jenkins-server:~$ `ssh -i /home/mike/.ssh/jenkins_key -l mike -p 8022 jenkins-server help`

==-i== flag (-i) is used to point to ==mike's== private SSH key. Remember, we have already added the public key in the Jenkins configuration.
==-l== is the login user which in our example is ==mike==
==-p== is the port which we found out in the previous step to be ==8022==

***Using the help command, find out the built-in command to safely restart Jenkins from the CLI.***
We can use the safe-restart built-in command:
```
safe-restart
    Safely restart Jenkins.
```
Try running the below command to restart jenkins from the CLI:
```js
mike@jenkins-server:~$ ssh -i /home/mike/.ssh/jenkins_key -l mike -p 8022 jenkins-server  safe-restart
mike@jenkins-server:~$
```