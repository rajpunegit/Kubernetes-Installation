curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt-get update

sudo apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu kubelet=1.12.2-00 kubeadm=1.12.2-00 kubectl=1.12.2-00 kubernetes-cni=0.6.0-00

sudo apt-mark hold docker-ce kubelet kubeadm kubectl kubernetes-cni

echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf

sudo sysctl -p


Only on master:
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
kubeadm join 10.142.0.2:6443 --token mcgw5f.1x3fuxqz9ovonq2n --discovery-token-ca-cert-hash sha256:f673c0518b8eb45fae7784306ae458892eb38331219b3d298eb00972de6261bd
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml


kubectl cluster-info | grep master
http://35.237.184.172:30002/ - sock-shop
http://35.237.184.172:31511/ - jenkins
get the external ip from linux cli: host myip.opendns.com resolver1.opendns.com

Troubleshooting:
sudo kubeadm reset -y
sudo systemctl stop kubelet 
sudo systemctl stop docker 
sudo rm -rf /var/lib/cni/ 
sudo rm -rf /var/lib/kubelet/* 
sudo rm -rf /etc/cni/ 
sudo ifconfig cni0 down 
sudo ifconfig flannel.1 down 
sudo ifconfig docker0 down


kubeadm join 10.142.0.2:6443 --token uet5xm.ybo86udrs0w7d8jd --discovery-token-ca-cert-hash sha256:f2b13272d6d0fcb4bb662e8158c253a99d30d69cbad4d5602b816d8b37e1c00d