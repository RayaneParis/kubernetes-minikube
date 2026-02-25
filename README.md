# kubernetes-minikube

Minikube is a tool that lets you run Kubernetes locally. 
minikube runs a single-node Kubernetes cluster on your personal computer (including Windows, macOS and Linux PCs) so that you can try out Kubernetes, or for daily development work.

## Docker installation

### installation for Mac, Windows 10 Pro, Enterprise, or Education

https://www.docker.com/get-started

Choose Docker Desktop

### installation for Windows home

https://docs.docker.com/docker-for-windows/install-windows-home/

## Kuberntes Minikube installation

https://minikube.sigs.k8s.io/docs/start/

Minikube provides a dashboard (web portal). Access the dashboard using the following command:

```
minikube dashboard
```

## Download this project

This project contains a web service coded in Java, but the language doesn't matter. This project has already been built and the binary version is there:

First of all, download and uncompress the project: https://github.com/charroux/kubernetes-minikube

You can also use git: `git clone https://github.com/charroux/kubernetes-minikube`

Then move to the sud directory with `cd kubernetes-minikube/myservice` where a DockerFile is.

## Test this project using Docker

Build the docker image:
```
docker build -t myservice .
```

Check the image:
```
docker images
```

Start the container:
```
docker run -p 4000:8080 -t myservice
```

8080 is the port of the web service, while 4000 is the port for accessing the container. Test the web service using a web browser: http://localhost:4000 It displays hello.

Ctrl-C to stop the Web Service.

Check the containerID:
```
docker ps
```

Stop the container:
```
docker stop containerID
```

## Publish the image to the Docker Hub

Retreive the image ID:
```
docker images
```

Tag the docker image: 
```
docker tag imageID yourDockerHubName/imageName:version
```

Example: `docker tag 1dsd512s0d myDockerID/myservice:1`

Login to docker hub: 
```
docker login
```
or
```
docker login http://hub.docker.com
```
or 
```
docker login -u username -p password
```

Push the image to the docker hub:
```
docker push yourDockerHubName/imageName:version
```

Example: `docker push myDockerID/myservice:1`

## Create a kubernetes deployment from a Docker image
Déja réalisé !

```
kubectl get nodes
```
```
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   76m   v1.35.1
```
```
kubectl create deployment myservice --image=efrei/myservice:1
```

The image used comes from the Docker hub: https://hub.docker.com/r/efrei/myservice/tags

But you can use your own image instead.

Check the pod:
```
kubectl get pods
```
```
rayanekhatim@MacBook-Pro-de-Rayane kubernetes-minikube % kubectl get pods
NAME                         READY   STATUS    RESTARTS   AGE
myservice-6577d5697f-4fpml   1/1     Running   0          4m20s
myservice-6577d5697f-rmbpn   1/1     Running   0          4m20s
```

Check if the state is running.

Get complete logs for a pods: 
```
kubectl describe pods
```
```
rayanekhatim@MacBook-Pro-de-Rayane kubernetes-minikube % kubectl describe pods
Name:             myservice-6577d5697f-4fpml
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Wed, 25 Feb 2026 16:02:03 +0100
Labels:           app=myservice
                  pod-template-hash=6577d5697f
Annotations:      <none>
Status:           Running
IP:               10.244.0.13
IPs:
  IP:           10.244.0.13
Controlled By:  ReplicaSet/myservice-6577d5697f
Containers:
  myservice:
    Container ID:   docker://ad37314653912e1b2482b4ebd04ca20797059f0df193dd3ea6664773d4b6d794
    Image:          rayaneparis/myservice:1
    Image ID:       docker-pullable://rayaneparis/myservice@sha256:26ad11374cd874240f8c069e1ecaec5f6b80508a7f3c2840d055125d9df38fb0
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Wed, 25 Feb 2026 16:02:04 +0100
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-dq6b9 (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       True 
  ContainersReady             True 
  PodScheduled                True 
Volumes:
  kube-api-access-dq6b9:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  4m49s  default-scheduler  Successfully assigned default/myservice-6577d5697f-4fpml to minikube
  Normal  Pulled     4m48s  kubelet            spec.containers{myservice}: Container image "rayaneparis/myservice:1" already present on machine and can be accessed by the pod
  Normal  Created    4m48s  kubelet            spec.containers{myservice}: Container created
  Normal  Started    4m48s  kubelet            spec.containers{myservice}: Container started


Name:             myservice-6577d5697f-rmbpn
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Wed, 25 Feb 2026 16:02:03 +0100
Labels:           app=myservice
                  pod-template-hash=6577d5697f
Annotations:      <none>
Status:           Running
IP:               10.244.0.12
IPs:
  IP:           10.244.0.12
Controlled By:  ReplicaSet/myservice-6577d5697f
Containers:
  myservice:
    Container ID:   docker://96d51dcf3606a0ad1b6fea452ae510b2eb3b27f94ebf08e8a1f1f14fef387d11
    Image:          rayaneparis/myservice:1
    Image ID:       docker-pullable://rayaneparis/myservice@sha256:26ad11374cd874240f8c069e1ecaec5f6b80508a7f3c2840d055125d9df38fb0
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Wed, 25 Feb 2026 16:02:03 +0100
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-p2h8w (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       True 
  ContainersReady             True 
  PodScheduled                True 
Volumes:
  kube-api-access-p2h8w:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  4m49s  default-scheduler  Successfully assigned default/myservice-6577d5697f-rmbpn to minikube
  Normal  Pulled     4m49s  kubelet            spec.containers{myservice}: Container image "rayaneparis/myservice:1" already present on machine and can be accessed by the pod
  Normal  Created    4m49s  kubelet            spec.containers{myservice}: Container created
  Normal  Started    4m49s  kubelet            spec.containers{myservice}: Container started

```

Retreive the IP address but notice that this IP address is ephemeral since a pods can be deleted and replaced by a new one.

Then retrieve the deployment in the minikube dashboard. 
Actually the Docker container is runnung inside a Kubernetes pods (look at the pod in the dashboard).
  
You can also enter inside the container in a interactive mode with:
```
kubectl exec -it podname -- /bin/bash
```

where podname is the name of the pods obtained with:
```
kubectl get pods
```

List the containt of the container with:
```
ls
```

Don't forget to exit the container with:
```
exit
```

## Expose the Deployment through a service

A Kubernetes Service is an abstraction which defines a logical set of Pods running somewhere in the cluster, 
that all provide the same functionality. 
When created, each Service is assigned a unique IP address (also called clusterIP). 
This address is tied to the lifespan of the Service, and will not change while the Service is alive.

## Expose HTTP and HTTPS routes from outside the cluster to services within the cluster

For some parts of your application (for example, frontends) you may want to expose a Service onto an external IP address, that’s outside of your cluster.

Kubernetes ServiceTypes allow you to specify what kind of Service you want. The default is ClusterIP.

Type values and their behaviors are:

* ClusterIP: Exposes the Service on a cluster-internal IP. Choosing this value makes the Service only reachable from within the cluster. This is the default ServiceType.
* NodePort: Exposes the Service on each Node’s IP at a static port (the NodePort). A ClusterIP Service, to which the NodePort Service routes, is automatically created. You’ll be able to contact the NodePort Service, from outside the cluster, by requesting NodeIP:NodePort.
* LoadBalancer: Exposes the Service externally using a cloud provider’s load balancer. NodePort and ClusterIP Services, to which the external load balancer routes, are automatically created.
* ExternalName: Maps the Service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record

## Expose HTTP and HTTPS route using NodePort
Déja réalisé !

```
kubectl expose deployment myservice --type=NodePort --port=8080
```

Retrieve the service address:
```
minikube service myservice --url
```

```
rayanekhatim@MacBook-Pro-de-Rayane kubernetes-minikube % minikube service myservice --url
http://127.0.0.1:63615
❗  Comme vous utilisez un pilote Docker sur darwin, le terminal doit être ouvert pour l'exécuter.
```
```
Hello
```
This format of this address is `NodeIP:NodePort`.

Test this address inside your browser. It should display hello again.

Look from the NodeIP and the NodePort in the minikube dashboard.

## Scaling and load balancing
Déja réalisé !

Check if the myservice deployment is running:

```
kubectl get deployments
```
```
rayanekhatim@MacBook-Pro-de-Rayane kubernetes-minikube % kubectl get deployments
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
myservice   2/2     2            2           59m
```
How many instance are actually running:

```
kubectl get pods
```
```
rayanekhatim@MacBook-Pro-de-Rayane kubernetes-minikube % kubectl get pods
NAME                         READY   STATUS    RESTARTS   AGE
myservice-6577d5697f-4fpml   1/1     Running   0          8m21s
myservice-6577d5697f-rmbpn   1/1     Running   0          8m21s
```
J'ai deux pods déja réalisés
Start a second instance:

```
kubectl scale --replicas=2 deployment/myservice
```
```
kubectl get deployments
```

and 

```
kubectl get pods
```

again

## Creating a Service of type LoadBalancer

Déja réalisé !

Check if the myservice deployment is running:

```
kubectl get deployments
```

If a service is running in front of the deployment you must delete this service first in ordre to create a new one of kind LoadBalancer. So retreive the service using:

```
kubectl get services
```
```
rayanekhatim@MacBook-Pro-de-Rayane kubernetes-minikube % kubectl get services
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP          82m
myservice    LoadBalancer   10.103.41.166   <pending>     8080:32508/TCP   31m
```

And delete it:
```
kubectl delete service serviceName
```
```
kubectl expose deployment myservice --type=LoadBalancer --port=8080
```
```
minikube service myservice --url
```
Test in your web browser

## Rolling updates


Rolling updates allow Deployments' update to take place with zero downtime by incrementally updating Pods instances with new ones.

To update the image of the application to version 2, use the set image subcommand, followed by the deployment name and the new image version:
```
kubectl set image deployments/my-deployment my-deployment=dockerHudId/my-image:v2
```

You can also confirm the update by running the rollout status subcommand:
```
kubectl rollout status deployments/my-deployment
```

To roll back the deployment to your last working version, use the rollout undo subcommand:
```
kubectl rollout undo deployments/my-deployment
```

```
rayanekhatim@MacBook-Pro-de-Rayane kubernetes-minikube % kubectl rollout status deployment/myservice
deployment "myservice" successfully rolled out
rayanekhatim@MacBook-Pro-de-Rayane kubernetes-minikube % kubectl rollout undo deployments/myservice
deployment.apps/myservice rolled back
```

## Create a deployment and a service using a yaml file

Yaml files can be used instead of using the command `kubectl create deployment` and `kubectl expose deployment`

The yaml file for the deployment: https://github.com/charroux/kubernetes-minikube/blob/main/myservice-deployment.yml

The yaml file for the node port service: https://github.com/charroux/kubernetes-minikube/blob/main/myservice-service.yml

The yaml file for the node port service: https://github.com/charroux/kubernetes-minikube/blob/main/myservice-loadbalancing-service.yml

Apply the deployment:
```
kubectl apply -f myservice-deployment.yml
```

Apply the node port service: 
```
kubectl apply -f myservice-service.yml
```

or 

Apply the service of type loadbalancer:
```
kubectl apply -f myservice-loadbalancing-service.yml
```

```
rayanekhatim@MacBook-Pro-de-Rayane kubernetes-minikube % kubectl apply -f myservice-deployment.yml
deployment.apps/myservice unchanged
rayanekhatim@MacBook-Pro-de-Rayane kubernetes-minikube % kubectl get deployments
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
myservice   1/1     1            1           66m
rayanekhatim@MacBook-Pro-de-Rayane kubernetes-minikube % kubectl get pods       
NAME                         READY   STATUS    RESTARTS   AGE
myservice-698b4db558-hbvn5   1/1     Running   0          4m14s
Then test if it works as expected.
```

# Routing rule to a service using Ingress

You can use Ingress to expose your Service. 
Ingress is not a Service type, but it acts as the entry point for your cluster. 
It lets you consolidate your routing rules into a single resource as it can expose multiple services under the same IP address.
Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. 
An Ingress may be configured to give Services externally-reachable URLs, load balance traffic, terminate SSL / TLS, and offer name-based virtual hosting.

## Set up Ingress on Minikube with the NGINX Ingress Controller

Enable the NGINX Ingress controller: 

```
minikube addons enable ingress
```
Verify that the NGINX Ingress controller is running:
```
kubectl get pods -n ingress-nginx
```

Create a Deployment and expose it as a NodePort (not a loadbalancer).

Check if it works.

A yaml file for ingress: https://github.com/charroux/kubernetes-minikube/blob/main/ingress.yml

```
kubectl apply -f ingress.yml
```

Retrieve the IP address of Ingress: 

```
kubectl get ingress
```

```
NAME                 CLASS    HOSTS                  ADDRESS        PORTS   AGE

example-ingress      nginx   myservice.info         192.168.64.2   80      18m
```

On Linux: edit the `/etc/hosts` file and add at the bottom values for: 

IngressAddress myservice.info

Where address is given by:
```
minikube ip
```

On Mac: edit the `/etc/hosts` file and add at the bottom values for: 

127.0.0.1 myservice.info


Then check in your Web browser: 

http://myservice.info/

On Windows : edit the `c:\windows\system32\drivers\etc\hosts` file, add 

`127.0.0.1 myservice.info`	

Enable a tunnel for Minikube:

```
minikube addons enable ingress-dns
```
```
minikube tunnel
```

Then check in your Web browser: 

http://myservice.info/


Create a second deployment and its service, then add a new route to the ingress.yml file.

## Delete resources

```
kubectl delete services myservice
```
```
kubectl delete deployment myservice
```

