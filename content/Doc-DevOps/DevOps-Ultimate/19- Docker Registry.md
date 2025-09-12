It's the central repository of all docker images.

image: docker.io/library/nginx
- docker.io : Registry (DNS name)
- library : user/account
- nginx : Image Repository
##### **Google's registry, where a lot of k8s related images are stored**
image: gcr.io/kubernetes-e2e-test-images/dnsutils
##### Deploy Private Registry
- AWS
- AZURE
- GCP

we need to login before To pull or push 
##### To run a container using a image from a private registry
1. Log in
- docker login private-registry.io
2. Run the app using private registry as part of the image name
- docker login private-registry.io/apps/internal-app

#### **Deploy Private Registry**
##### ***What if we're running your app on premise and do not have a private registry?***
1. The name of the image is registry and it exposes the API on port 5000
	- ***docker run -d -p 5000:5000 --name registry:2***
2. Use the docker ***image tag*** cmd to tag the image with the private registry URL in it. In this case is running in the same docker host
	- ***docker image tag my-image localhost:5000/my-image***
3. Then we can ***push*** it to the docker registry.
	- ***docker push localhost:5000/my-image***
4. If we are ***within*** this network using either localhost
	- ***docker pull localhost:5000/my-image***
5. If we are in a ***different host***, we put the ***IP or domain name*** of my docker host environment.
	- ***docker pull 192.168.56.100:5000/my-image***
