1. Develop -> code.py
2. Build -> ./build.sh ; app.exe ; ./app
3. Deploy -> ./app

#### **CI/CD PIPELINE**
- Development -> code
- Build -> Container image
- Test -> Container
- Production -> Container, it could have many containers and K8S we can manage the Container Orquestration 

***K8S*** -> it manage the Container Orquestration 

***Terraform*** -> Infrastructure as Code (IaC) (.tf)
It manages all the Pipeline as Build, Test, and Production.
It is for provisioning infrastructure

***Ansible*** -> Use for post configuration activities such as installing the software and configuring them on those servers. (.yml)

Terraform and Ansible goes to the source code Repository such as GitHub or GitLab

***Prometheus*** -> Collects information or metrics from the different servers and stores it centrally.  
It maintains and qe can be able to see the CPU utilization on these servers, the memory consumption, monitor the processes, identify what process is causing higher consumption, etc

***Grafana*** ->  it helps make sense out of the data collected by Prometheus by visualizing them graphically it into charts.

***The Infinity Cicle***
Code -> Build -> Test -> Release -> Deploy -> Operate -> Monitor -> Feedback