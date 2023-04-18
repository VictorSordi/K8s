sudo apt-get update && apt-get install apt-transport-https ca-certificates curl software-properties-common gnupg2 -y
  
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt-get update && apt-get install containerd.io docker-ce docker-ce-cli -y

nano /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
} 

sudo mkdir -p /etc/systemd/system/docker.service.d

sudo systemctl daemon-reload
sudo systemctl restart docker

sudo apt-get install apt-transport-https curl -y
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"

sudo apt install kubeadm

sudo kubeadm version

sudo swapoff -a

rm /etc/containerd/config.toml

systemctl restart containerd

kubeadm init --control-plane-endpoint="192.168.15.197:6443" --upload-certs --apiserver-advertise-address=192.168.15.195 --pod-network-cidr=10.244.0.0/16  


  kubeadm join 192.168.15.197:6443 --token cshtz7.2xm3o1pvkacd68tc \
        --discovery-token-ca-cert-hash sha256:f07d4286ed14946416e800d4f7894405c5d56f82bade95e5882d995d5e7c6022 \
        --control-plane --certificate-key 2ef78ce3592c8be6b4a9a582fd948d6ec0df125dc5bab0d644f5163d42147574

kubeadm join 192.168.15.197:6443 --token cshtz7.2xm3o1pvkacd68tc \
        --discovery-token-ca-cert-hash sha256:f07d4286ed14946416e800d4f7894405c5d56f82bade95e5882d995d5e7c6022 
