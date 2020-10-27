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
Verify swap<br />
$free -m<br />
Upgrade all libraries<br />
$sudo apt-get update <br />
Install & Enable Docker<br />
$sudo apt install docker.io -y<br />
$sudo systemctl enable docker<br />
  
Get the gpg keys for Kubeadm
$curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add<br />
$sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"<br />
  
Install Kubeadmin with specific K8s version or by default, it will install the latest version<br />
Let us install K8s V1.18<br />
$sudo apt install kubeadm=1.18 -y<br />

```
  

### Let us now install the Control Plane on the MASTER NODE<br />

```
Run only on Master node<br />
$sudo kubeadm init<br />
Once the Control Plane comes up without any errors, let us copy config files<br />
$mkdir $HOME/.kube<br />
$sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config<br />
$sudo chown $(id -u):$(id -g) $HOME/.kube/config<br />
  
```
