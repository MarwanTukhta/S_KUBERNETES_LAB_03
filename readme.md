1- How many static pods exist in this cluster in all namespaces?<br>
8
```
$ kubectl get pods --all-namespaces -o custom-columns=NAME:.metadata.name,CONTROLLER:.metadata.ownerReferences[].kind,NAMESPACE:.metadata.namespace
NAME                               CONTROLLER   NAMESPACE
coredns-64897985d-ll7pd            ReplicaSet   kube-system
elasticsearch-7h2pm                DaemonSet    kube-system
etcd-minikube                      Node         kube-system
kube-apiserver-minikube            Node         kube-system
kube-controller-manager-minikube   Node         kube-system
kube-proxy-rqm49                   DaemonSet    kube-system
kube-scheduler-minikube            Node         kube-system
storage-provisioner                <none>       kube-system
```

2-On which nodes are the static pods created currently?
<br>
minikube

3- What is the path of the directory holding the static pod definition files?
<br>
etc/coredns/Corefile

4- Create a static pod named static-busybox that uses the busybox image and the command sleep 1000
<br>
check static-busybox-pod.yaml
```
$ kubectl apply -f static-busybox-pod.yaml
pod/static-busybox created
```

5- Edit the image on the static pod to use busybox:1.28.4
<br>
check static-busybox-pod.yaml
```
$ kubectl apply -f static-busybox-pod.yaml 
pod/static-busybox configured
```

6- How many ConfigMaps exist in the environment?
<br>
10
```
$ kubectl get cm --all-namespaces
NAMESPACE         NAME                                 DATA   AGE  
default           kube-root-ca.crt                     1      5d22h
kube-node-lease   kube-root-ca.crt                     1      5d22h
kube-public       cluster-info                         1      5d22h
kube-public       kube-root-ca.crt                     1      5d22h
kube-system       coredns                              1      5d22h
kube-system       extension-apiserver-authentication   6      5d22h
kube-system       kube-proxy                           2      5d22h
kube-system       kube-root-ca.crt                     1      5d22h
kube-system       kubeadm-config                       1      5d22h
kube-system       kubelet-config-1.23                  1      5d22h
```

7- Create a new ConfigMap  Use the spec given below.
       ConfigName Name: webapp-config-map
       Data: APP_COLOR=darkblue
```
$ kubectl create cm webapp-config-map --from-literal=APP_COLOR=darkblue
configmap/webapp-config-map created
```

8- Create a  webapp-color POD with nginx image and use the created ConfigMap
<br>
check webapp-color-pod.yaml

9- How many Secrets exist on the system?
```
$ kubectl get secrets
NAME                  TYPE                                  DATA   AGE 
default-token-8cc2v   kubernetes.io/service-account-token   3      6d3h
```

10- How many secrets are defined in the default-token secret?
<br>
1

11- create a POD called db-pod with the image mysql:5.7 then check the POD status
<br>
check db-pod.yaml

12- why the db-pod status not ready
<br>
got error `CreateContainerConfigError` because mysql image require enviroment variables to be set


13- Create a new secret named db-secret with the data given below.
	- Secret Name: db-secret
	- Secret 1: MYSQL_DATABASE=sql01
	- Secret 2: MYSQL_USER=user1
	- Secret 3: MYSQL_PASSWORD=password
	- Secret 4: MYSQL_ROOT_PASSWORD=password123
<br>
check db-pod.yaml

14- Configure db-pod to load environment variables from the newly created secret. Delete and recreate the pod if required.
<br>
check db-pod.yaml

15- Create a multi-container pod with 2 containers.
	- Name: yellow
	- Container 1 Name: lemon 
	- Container 1 Image: busybox
	- Container 2 Name: gold 
	- Container 2 Image: redis

<br>
check yellow-pod.yaml


16- Create a pod red with redis image and use an initContainer that uses the busybox image and sleeps for 20 seconds
<br>
check red-pod.yaml

17- Create a Persistent Volume with the given specification.
	- Volume Name: pv-log
	- Storage: 100Mi
	- Access Modes: ReadWriteMany
	- Host Path: /pv/log
<br>
check pv-log-volume.yaml

18- Create a Persistent Volume Claim with the given specification.
	- Volume Name: claim-log-1
	- Storage Request: 50Mi
	- Access Modes: ReadWriteMany
<br>
check pv-claim-log.yaml

19- Create a webapp pod to use the persistent volume claim as its storage.
	- Name: webapp
	- Image Name: nginx
	- Volume: PersistentVolumeClaim=claim-log-1
	- Volume Mount: /var/log/nginx
<br>
check web-app-pod.yaml

20- Create a pod named volume-share-datacenter. 
For first container, use image centos:latest, container should be named as volume-container-datacenter-1, and run a command '/bin/bash', '-c' and 'sleep 10000'. Volume volume-share should be mounted at path /tmp/news.
For second container, use image centos:latest, container should be named as volume-container-datacenter-2, and run a command '/bin/bash', '-c' and 'sleep 10000'. Volume volume-share should be mounted at path /tmp/cluster.
Volumes to be named as volume-share and use emptyDir: { }.
After creating the pod, exec into the first container volume-container-datacenter-1, and create a file news.txt with content Welcome from datacenter-1! under the mount path of first container  /tmp/news.
The file news.txt should be present under the mounted path /tmp/cluster of second container volume-container-datacenter-2 as they are using shared volumes.

<br>
check volume-share-datacenter.yaml


21- Create a pod named webserver.
Create an emptyDir volume name: shared-logs.
Create two containers from nginx and ubuntu images with latest tag only and remember to mention tag i.e nginx:latest, nginx container name should be nginx-container and ubuntu container name should be sidecar-container on webserver pod.
Add command on sidecar-container "sh","-c","while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep 30; done"
Mount volume /var/log/nginx on both containers, all containers should be up and running.
<br>
check webserver-pod.yaml

22- Create a new service account with the name pvviewer. Grant this Service account access to list all PersistentVolumes in the cluster by creating an appropriate cluster role called pvviewer-role and ClusterRoleBinding called pvviewer-role-binding.
<br>
check pv-viewer-account.yaml

23- Create a pod named print-envars-greeting.
	1- Configure spec as, the container name should be print-env-container and use bash image.
	2- Create three environment variables:
		-a GREETING and its value should be Welcome to
		-b COMPANY and its value should be DevOps
		-c GROUP and its value should be Industries
	4- Use command to echo ["$(GREETING) $(COMPANY) $(GROUP)"] message.
	5- You can check the output using <kubctl logs -f [ pod-name ]> command.
<br>
