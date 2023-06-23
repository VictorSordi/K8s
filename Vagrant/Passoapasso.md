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

kubeadm init --control-plane-endpoint="192.168.15.198:6443" --upload-certs --apiserver-advertise-address=192.168.15.195 --pod-network-cidr=10.244.0.0/16  

  kubeadm join 192.168.15.198:6443 --token uq1gc7.r8iaovtjp8v8tn3t \
        --discovery-token-ca-cert-hash sha256:ecf44ed9f8328e51eccdc53dda0154a7b421108d4b6180c6839fa74cb858f616 \
        --control-plane --certificate-key 398cbd732433cfe1dc8ec3784cc27be68ca2296a1264144fd9b3404297dfe527

kubeadm join 192.168.15.198:6443 --token uq1gc7.r8iaovtjp8v8tn3t \
        --discovery-token-ca-cert-hash sha256:ecf44ed9f8328e51eccdc53dda0154a7b421108d4b6180c6839fa74cb858f616 