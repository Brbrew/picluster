# Master Node Setup

pi_cluster
================================

-- Update/Upgrade
sudo apt-get update && sudo apt-get upgrade -y && sudo apt-get autoremove && sudo apt-get autoclean

-- Install haveged
sudo apt-get install haveged -y

-- Install pre-req
sudo apt-get install apt-transport-https ca-certificates software-properties-common -y

-- Install Docker
curl -fsSL get.docker.com -o get-docker.sh && sh get-docker.sh



-- Give the ‘pi’ user the ability to run Docker.
sudo usermod -aG docker pi

-- Import Docker CPG key.
sudo curl https://download.docker.com/linux/raspbian/gpg
sudo curl -fsSL https://download.docker.com/linux/raspbian/gpg | sudo apt-key add -


-- Setup the Docker Repo.
nano /etc/apt/sources.list

-- Add the following line and save:  
deb https://download.docker.com/linux/raspbian/ stretch stable

-- Update anything remaining
sudo apt-get update && sudo apt-get upgrade -y && sudo apt-get autoremove && sudo apt-get autoclean

-- Add root
sudo passwd root

-- Start the Docker service.
systemctl start docker.service



docker info



===============================================
https://www.hanselman.com/blog/HowToBuildAKubernetesClusterWithARMRaspberryPiThenRunNETCoreOnOpenFaas.aspx
https://github.com/teamserverless/k8s-on-raspbian/blob/master/GUIDE.md


-- Disable Swap
sudo dphys-swapfile swapoff &&  sudo dphys-swapfile uninstall && sudo update-rc.d dphys-swapfile remove


sudo nano /boot/cmdline

-- add 
cgroup_enable=cpuset cgroup_enable=memory

-- Install Kubernetes
sudo curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -


echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list 

sudo apt-get update -q 
sudo apt-get install -qy kubeadm 


-- initialized main node
sudo kubeadm init --apiserver-advertise-address=10.10.10.10





===============================================
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 10.10.10.10:6443 --token 7cdb4x.u60iixr1sfpog695 \
    --discovery-token-ca-cert-hash sha256:d7f59f72d6de4c29b04b1b5abe4bf038056dbc25f26d58aa46747e57f0d186ec 
