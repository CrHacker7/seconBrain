##### First install kubectl and then minikube.
1. curl (download bynaries)
	1. curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
	2. install kubectl:
	   sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
2. chmod +x ./kubectl  (make binary executable)
3. sudo mv ./kubectl  /usr/local/bin/kutectl (move it in to your PATH)
4. kubectl version
Another way to install it is from a apt-get
##### Before begin:
Check the virtualization, if empty, change it in the BIOS
- grep -E --color 'vmx|svm' /proc/cpuinfo
##### Install a Hypervisor
- KVM, which also uses QEMU
- VirtualBox

> [!warning]
> If you have Docker in your host installed, we can skip this step. But it can result in security and data loss issues.

It's better the VM option in case we mess up sth on our system and we need to restart, it's easy to get rid of the VM and restart again. We can also make a restore with snapshot before doing major changes.

So after installing VM, we provision a cluster using minikube, it will automatically create a virtyal machine as required.
##### Install the minikube utility
1. curl -Lo ... && chmod +x  minikube
	1. curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
	2. sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
2. Verify the /usr/local/bin/  is in our PATH
	1. ls -ld /usr/local/bin/  
	2. if not: `sudo mkdir -p /usr/local/bin/`
3. Install it at the location /usr/local/bin/ 
	1. `sudo install minikube /usr/local/bin/` 
4. Specify the driver of virtualization --drive=\<name>
	1. `minikube start --drive=virtualbox
5. Check the VM, we'll have one by the name *minikube* in running state
6. `minikube status`
7. `minikube start`
##### What's next?
1. Check if kubectl are working
	1. `kubectl get nodes'
2. Deploy using an existing image named *echoserver*
	1. `kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.10`
3. Verify the deployment
	1. `kubectl get deployments`
4. Expose it as a service
	1. `kubectl expose deployment hello-minikube --type=NodePort --port=8080`
5. Get the URL of the exposed Service
	1. `minikube service hello-minikube --url
	2. copy the url into the browser, proof it works!
6. Delete Service
	1. `kubectl delete services hello-minikube`
7. Delete Deployment
	1. `kubectl delete deployment hello-minikube`
8. Check if it's deleted, not showing in the browser anymore.
	1. `kubectl get pods`
