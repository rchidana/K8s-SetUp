# K8s-SetUp on Ubuntu 18.04 Servers

Document to set up vanilla Kubernetes set-up on Linux Servers running Ubuntu 18.04 version

### Ports to be opened on Kubernetes Master (Control Plane)

TCP	Inbound	6443	Kubernetes API server	All<br />
TCP	Inbound	2379-2380	etcd server client API	kube-apiserver, etcd<br />
TCP	Inbound	10250	Kubelet API	Self, Control plane<br />
TCP	Inbound	10251	kube-scheduler	Self<br />
TCP	Inbound	10252	kube-controller-manager	Self<br />

### Ports to be opened on Worker Nodes (Control Plane)

TCP	Inbound	10250	Kubelet API	Self, Control plane<br />
TCP	Inbound	30000-32767	NodePort Services†	All<br />

### Installation Steps on both Master & Worker Nodes

```
Turn off swap
$sudo swapoff -a
Verify swap
$free -m
Upgrade all libraries
$sudo apt-get update 

Install & Enable Docker
$sudo apt install docker.io -y
$sudo systemctl enable docker

Start Docker in case its not already started
$sudo systemctl start docker
$sudo systemctl status docker

Get the gpg keys for Kubeadm
$curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
$sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"

Install Kubeadmin with specific K8s version or by default, it will install the latest version
Let us install K8s V1.18
$sudo apt install kubeadm=1.18.0-00 kubelet=1.18.0-00 kubectl=1.18.0-00 -y
In case you do not want unwanted updates happening
$sudo apt-mark hold docker kubelet kubeadm kubectl
Verify kubeadm version
$kubeadm version

In order to set a unique name for the master node
$sudo hostnamectl set-hostname master-node

```
  

### Let us now install the Control Plane on the MASTER NODE

```
Run only on Master node
kubeadm important flags --apiserver-advertise-address IP_ADDRESS_OF_VM --pod-network-cidr CIDR_ADDRESS_RANGE
$sudo kubeadm init --pod-network-cidr=10.244.0.0/16

Once the Control Plane comes up without any errors, let us copy config files
$mkdir $HOME/.kube
$sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
$sudo chown $(id -u):$(id -g) $HOME/.kube/config
  
```

### By default, K8s does not install any network interfaces and we need to install at least one CNI plugin in order to enable communication within the cluster

```
$kubectl get nodes

Let us install Flannel Network provider

$kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

Check the status of all Pods and wait till all required Pods get to RUNNING STATE

$kubectl get pods --all-namespaces

$kubectl get nodes

```

