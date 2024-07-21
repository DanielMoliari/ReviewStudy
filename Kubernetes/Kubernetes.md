


<iframe width="914" height="514" src="https://www.youtube.com/embed/zEOeukcJl6E" title="TUTORIAL COMPLETO SOBRE KUBERNETES PARA INICIANTES!" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Container Runtime

Container Engine

Kubelet/Docker -> containerd/dockerd -> runc -> kernel Linux

Cluster Kubernetes {
Control Plane {
	etcd
	Scheduler
	Controller Manager
	API Server
} -> EKS, AKS
Workers {
	Kubelet
	Kube Proxy
}
}

Ports:

Control Plane {
	API Server -> 6443 -> TCP
	etcd Server -> 2379 - 2380
	Kubelet -> 10250
	Scheduler -> 10251
	Controller Manager -> 10252
}

Workers {
	Kubelet -> 10250
	NodePort -> 30000 - 32767
}

Endpoint -> Services(NodePort, LoadBalancer, ClusterIP) -> Deployment -> ReplicaSet -> Pods(Menor Unidade do Kubernet, podendo conter um ou mais containers)

alias k=kubectl

k get nodes

k describe nodes

k get pods -A -o wide

k get pods -n kube-system

k get namespaces

k run nginx --image nginx

k describe pods nginx

k delete pods nginx

k run nginx --image nginx --port 80

k expose pods nginx

k get service

k delete service nginx

k expose pod nginx --type NodePort

k get svc

k logs -f nginx

k exec -ti nginx bash

k create namespace giropops

k create deployment strigus --image nginx --replicas 10

k get depoloyment.apps

k delete deployment strigus

k create deployment strigus --image nginx --replicas 10 -n giropops

k get deploy

k get deploy -n giropops

k get pods

k get pods -n giropops

k create deployment strigus --image nginx --replicas 10 -n giropops --port 80

k expose deployment -n giropops strigus

k get svc -n giropops

k create deployment strigus --image nginx --replicas 10 -n giropops --port 80 --dry-run=client -o yaml

k get pods nginx -o yaml

k create deployment strigus --image nginx --replicas 10 -n giropops --port 80 --dry-run=client -o yaml > meu-deployment.yaml

vim meu-deployment.yaml

k apply -f meu-deployment.yaml

k run nginx --image nginx --port 80 --dry-run=client -o yaml

k expose deployment strigus --type NodePort --dry-run=client -o yaml

kubectl expain service/ingress/pod

k get endpoints

k get endpoints -n giropops




<iframe width="914" height="514" src="https://www.youtube.com/embed/tRbFs3CCyPQ" title="Iniciando com Kubernetes: Tutorial passo a passo" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


Kubernetes

Necessidades:

- Variáveis de ambientes
- Gerenciamento de senhas / secrets
- Escolher os recursos computacionais que a minha aplicação roda
- Health check
- Load balacing
- SSL / TLS
- Domínio (www) -> Determinado serviço
- Estratégias de deploy (Blue/Green, Canary, etc)
- Storage
- Service Discover / DNS

Estrutura de uma aplicação que roda no K8S

Service -> Deployment -> Replica Set - N de Pods -> Pod: Net label: nginx -> App


kind create cluster

kubectl cluster-info --context kind-kind

kubectl get nodes

pod.yaml -> 

{
apiVersion: v1
kind: Pod
metadata:
	name: nginx
	labels:
		name: nginx
spec:
	containers:
	- name: nginx
	image: nginx:latest
	ports:
		- containerPort: 80
}

kubectl apply -f pod.yaml

kubectl delete pod nginx

replicaset.yaml -> 

{
apiVersion: apps/v1
kind: ReplicaSet
metadata:
	name: nginx-replicaset
spec:
	replicas: 10
	selector:
		matchLabels:
			app: nginx
		template:
			metadata:
				labels:
					app: nginx
			spec:
				containers:
				- name: nginx
				image: nginx: latest
				ports:
				- containerPort: 80
}

kubectl apply -f replicaset.yaml

kubectl get rs

kubectl get pods

kubectl port-forward pod/nginx-replicaset-6ns2k 8080:80

deployment -> 

{
apiVersion: apps/v1
kind: Deployment
metadata:
	name: nginx-deployment
spec:
	replicas: 10
	selector:
		matchLabels:
			app: nginx
		template:
			metadata:
				labels:
					app: nginx
			spec:
				containers:
				- name: nginx
				image: nginx: latest
				ports:
				- containerPort: 80
}

kubectl apply -f deployment.yaml

kubectl get deployment

kubectl get rs

kubectl get pods

service.yaml ->

{
apiVersion: v1
kind: Service
metadata:
	name: nginx-service
spec:
	type: LoadBalancer 
	selector:
		app: nginx
	ports:
		-port: 80
		targetPort: 80
}

User -> Service (label: app: nginx) -> Pod: Net -App- label: nginx, Pod: Net -App- label: nginx, Pod: Net -App- label: nginx

kubectl apply -f service.yaml

kubectl get svc

kubectl port-forward svc/nginx-service 8080:80

