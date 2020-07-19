# Kubernetes Example

> Kubernetes setup guide

## Setup
Run the following commands on your all cluster nodes

1. Setup docker
	```
	sudo apt-get update
	sudo apt-get install docker.io
	sudo systemctl start docker
	sudo systemctl enable docker
	```
2. Turn off swap memory
	```
	sudo swapoff -a
	```
3. Setup kubernetes
	```
	curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
	sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
	sudo apt-get install kubeadm kubelet kubectl
	sudo apt-mark hold kubeadm kubelet kubectl
	```

## Getting Ready
1. Initialize your control plane (master node)
	```
	sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=<MASTER_NODE_IP>
	mkdir -p $HOME/.kube
	sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
	sudo chown $(id -u):$(id -g) $HOME/.kube/config
	```
2. Make kubernetes config directory and put the config files there
	```
	mkdir -p $HOME/.kube
	sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
	sudo chown $(id -u):$(id -g) $HOME/.kube/config
	```
3. Setup the flannel network
	```
	sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
	```
4. Join to the cluster from other nodes
	```
	sudo kubeadm join <MASTER_NODE_IP>:6443 --token <TOKEN> \
    	--discovery-token-ca-cert-hash <DISCOVERY_TOKEN>
	```
5. Make sure all nodes are connected & ready by running the command in control plane
	```
	kubectl get nodes
	```
6. Run a simple app on kubernetes  
	```
	# Create the deployment pods
	sudo kubectl apply -f https://raw.githubusercontent.com/sohelamin/k8s-example/master/k8s/deployment.yml
	# Create the service to expose outside the cluster
	sudo kubectl apply -f https://raw.githubusercontent.com/sohelamin/k8s-example/master/k8s/service.yml
	```
7. Now you have a NodePort eg. 3xxxx and you can get it by the command
    ```kubectl get svc```
8. Visit on browser http://<NODE_IP>:<NODE_PORT>

### References
https://phoenixnap.com/kb/install-kubernetes-on-ubuntu
https://learnk8s.io/nodejs-kubernetes-guide
