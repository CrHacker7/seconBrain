## What is a Pipeline?
Pipeline -> The task you are trying to accomplish
agent any -> Build Agent (the place where we run the pipe)
stage (UAT) -> There are many kind of stage like 
1. Prod stage
2. QA stage
steps -> The work that's been done in the pipeline.

Multi stage pipelines 
1. Dev 
2. Staging 
3. Prod 

## LAB
***What is a Jenkinsfile?***
- A Jenkinsfile is a text file that contains the definition of a Jenkins Pipeline.
***Install Pipeline Jenkins plugin?***
1. Go to Manage Jenkins.
2. Click on Plugins.
3. Under Available, search for Pipeline plugin.
4. Select it and install.
5. Once installed click on Restart Jenkins when installation is complete and no jobs are running.

***Create a pipeline job named hello-world, it should just echo the Hello World string.***
1. On the left side click on New Item.
2. Write the job name hello-world.
3. Select Pipeline job.
4. Under Pipeline section keep selected Pipeline script as Definition and add below given code in the Script
```groovy
pipeline {
    agent any
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
    }
}
```
5. Finally save the job.

***We have a pipeline job named go-test but its incomplete.
Complete the same by adding the required stages/steps as per details mentioned below:
1. **Clone a git repository.**
**git 'https://github.com/kodekloudhub/go-webapp-sample.git'**
2. **Run a shell command go test ./...**
**sh 'go test ./...'**
**Note: You can name the stages as per your choice.**
1. Click on go-test job.
2. Open the job configuration by clicking on Configure button.
3. Under the pipeline section, complete the Script and the final code should look like as below
```groovy
pipeline {
    agent {
        label {
            label 'master'
            customWorkspace "${JENKINS_HOME}/${BUILD_NUMBER}/"
        }
    }
    environment {
        Go111MODULE='on'
    }
    stages {
        stage('Test') {
            steps {
                git 'https://github.com/kodekloudhub/go-webapp-sample.git'
                sh 'go test ./...'
            }
        }
    }
}
```
***Now that you have built the docker image so let's modify the go-test pipeline job to deploy an app using this docker image. Find below more details:
Add an another step to run a docker container using the docker image you are building in this pipeline itself. Make sure to map container port 8000 with docker host port 8090. This is the step you can add:***
`sh "docker run -p 8090:8000 -d adminturneddevops/go-webapp-sample"`
```groovy
pipeline {
    agent {
        label {
            label 'master'
            customWorkspace "${JENKINS_HOME}/${BUILD_NUMBER}/"
        }
    }
    environment {
        Go111MODULE='on'
    }
    stages{
        stage('prueba'){
            steps {
                git 'https://github.com/kodekloudhub/go-webapp-sample.git'
                
            }
        }
         stage('docker') {
            steps {
                script{
                    image = docker.build("adminturneddevops/go-webapp-sample")
                     sh "docker run -p 8090:8000 -d adminturneddevops/go-webapp-sample"
                }
            }
        }
    }
}
```
