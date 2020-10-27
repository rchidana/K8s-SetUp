# K8s-SetUp on Ubuntu 18.04 Servers

Document to set up vanilla Kubernetes set-up on Linux Servers running Ubuntu 18.04 version

### Ports to be opened on Kubernetes Master (Control Plane)

TCP	Inbound	6443*	Kubernetes API server	All
TCP	Inbound	2379-2380	etcd server client API	kube-apiserver, etcd
TCP	Inbound	10250	Kubelet API	Self, Control plane
TCP	Inbound	10251	kube-scheduler	Self
TCP	Inbound	10252	kube-controller-manager	Self

### Ports to be opened on Worker Nodes (Control Plane)

TCP	Inbound	10250	Kubelet API	Self, Control plane
TCP	Inbound	30000-32767	NodePort Services†	All

### Installation Steps on both Master & Worker Nodes

  # Turn off swap
  >sudo swapoff -a
  # Verify swap
  >free -m
  #Upgrade all libraries
  >sudo apt-get update 
  #Install & Enable Docker
  >sudo apt install docker.io -y
  >sudo systemctl enable docker
  
  # Get the gpg keys for Kubeadm
  >curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
  >sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
  
  # Install Kubeadmin with specific K8s version or by default, it will install the latest version
  # Let us install K8s V1.18
  >sudo apt install kubeadm=1.18 -y
  

### Let us now install the Control Plane on the MASTER NODE

  # Run only on Master node
  >sudo kubeadm init
  # Once the Control Plane comes up without any errors, let us copy config files
  >mkdir $HOME/.kube
  >sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
  >sudo chown $(id -u):$(id -g) $HOME/.kube/config
  
  
