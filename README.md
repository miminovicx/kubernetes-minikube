# kubernetes-minikube

## Installation
- Docker
- Kubernetes minikube https://minikube.sigs.k8s.io/docs/start/

## Project
The project is imported here in myService folder.
Testing it using : 
- ./gradlew build (gradlew build (sous windows))

## Steps :
1- Get a source code
2- Create its image through Docker then the container
3- Create the pod with Kubernetes
  3.1- Create a deployment from a Docker image
  3.1- Expose the deployment

  3.2- Create a .yaml file
  3.2- Execute the file to expose

## Docker
### Build and start 
- Building the docker image : docker build -t myservice .
- Check the image: docker images

- Start the container: docker run -p 4000:8080 -t myservice
8080 is the port of the web service, while 4000 is the port for accessing the container.

- We can test with http://localhost:4000

Additionnal : 
Check the containerID: docker ps
Stop the container: docker stop containerID

### Publish image
Retreive the image ID: docker images

Tag the docker image: docker tag imageID yourDockerHubName/imageName:version

Login to docker hub:
docker login or
docker login http://hub.docker.com or
docker login -u username -p password

Push the image to the docker hub: 
docker push yourDockerHubName/imageName:version

## Kubernetes deployment
### From a Docker image
kubectl get nodes

kubectl create deployment myservice --image=miminovicx/myservicename:version 

Check the pod: kubectl get pods

Get complete logs for a pods: kubectl describe pods

U can deploy it as a NodePort :
kubectl expose deployment myservice --type=NodePort --port=8080

U can deploy it as a LoadBalancer : 
kubectl scale --replicas=2 deployment/myservice
kubectl expose deployment myservice --type=LoadBalancer --port=8080
launch : minikube service myservice --url


### From a .yaml file
Yaml files can be used instead of using the command :
kubectl create deployment 
kubectl expose deployment

Apply deployment : 
kubectl apply -f myservice-deployment.yml

Apply the node port service: kubectl apply -f myservice-service.yml
or
Apply the service of type loadbalancer: kubectl apply -f myservice-loadbalancing-service.yml

### Set up Ingress on Minikube with the NGINX Ingress Controller

Enable the NGINX Ingress controller:
minikube addons enable ingress

Verify that the NGINX Ingress controller is running:
kubectl get pods -n kube-system

Create a Deployment and expose it as a NodePort (not a loadbalancer).
Check if it works.


kubectl apply -f ingress.yml

Retrieve the IP address of Ingress:
kubectl get ingress

On Windows : edit the c:\windows\system32\drivers\etc\hosts file, add
127.0.0.1 myservice.info

Enable a tunnel for Minikube:
minikube addons enable ingress-dns
minikube tunnel

Then check in your Web browser:
http://myservice.info/


Delete
kubectl delete services myservice

kubectl delete deployment myservice
