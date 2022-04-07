# Deploying Kubenernetes Cluster in VMWare Player

## 1 - Deployed 3 Ubuntu VMs in VMWare Player

        2 vCPU
        2 GB RAM
        35 GB HD

## 2 - Disabled Swap

        sudo swapoff -a
        sudo nano /etc/fstab (commented swap line)

## 3 - Installed Docker (https://docs.docker.com/engine/install/ubuntu/)
        sudo apt-get update

        sudo apt-get install \
        ca-certificates \
        curl \
        gnupg \
        lsb-release

        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

        echo \
        "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
        $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

        sudo apt-get update
        
        sudo apt-get install docker-ce docker-ce-cli containerd.io

## 4 - Gave privileges to normal user "k8s"

        sudo usermod -aG docker k8s

## 5 - Install Kubernetes (https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

        sudo apt-get update

        sudo apt-get install -y apt-transport-https ca-certificates curl

        sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

        echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

        sudo apt-get update
        
        sudo apt-get install -y kubelet kubeadm kubectl
        
        sudo apt-mark hold kubelet kubeadm kubectl                

## 6 - Letting Iptables see bridged traffic (https://www.cloudsigma.com/how-to-install-and-use-kubernetes-on-ubuntu-20-04/)

        lsmod | grep br_netfilter

        sudo modprobe br_netfilter

        sudo sysctl net.bridge.bridge-nf-call-iptables=1
        
## 7 - Changing Docker Cgroup (https://www.cloudsigma.com/how-to-install-and-use-kubernetes-on-ubuntu-20-04/)

        cat <<EOF | sudo tee /etc/docker/daemon.json
        { "exec-opts": ["native.cgroupdriver=systemd"],
        "log-driver": "json-file",
        "log-opts":
        { "max-size": "100m" },
        "storage-driver": "overlay2"
        }
        EOF

        sudo systemctl enable docker
        
        sudo systemctl daemon-reload
        
        sudo systemctl restart docker


## 8 - Initializing Kubernetes
        
        # Only in Master Node #
        kubeadm init
        
        # Only in Master Node #
        mkdir -p $HOME/.kube
        sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
        sudo chown $(id -u):$(id -g) $HOME/.kube/config

        # Only in Master Node # (https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#install)
        kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"

## 9 - Allow Firewall on all VMs
        
        # All Nodes #
        sudo ufw allow 6443
        sudo ufw allow 6443/tcp


## 10 - Join Worker Nodes
        
        kubeadm join 192.168.15.201:6443 --token sbj8nz.hmuj9py0yc6k6tz0 --discovery-token-ca-cert-hash sha256:bdc37e2a97fc92d6de1a44531507d0e8d5228e76856ba700c476ab2a27792444




















8 - Initializing Kubernetes (Only in Master Node) (https://www.cloudsigma.com/how-to-install-and-use-kubernetes-on-ubuntu-20-04/)

        # Only in Master Node #
        sudo kubeadm init --pod-network-cidr=10.244.0.0/16        

        # Only in Master Node #
        mkdir -p $HOME/.kube
        sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
        sudo chown $(id -u):$(id -g) $HOME/.kube/config

9 - Deploying a Pod Network (https://www.cloudsigma.com/how-to-install-and-use-kubernetes-on-ubuntu-20-04/)

        # All Nodes #
        sudo ufw allow 6443
        sudo ufw allow 6443/tcp

        # Only in Master Node #
        kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
        kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/k8s-manifests/kube-flannel-rbac.yml

10 - Joining Worker Nodes (https://www.cloudsigma.com/how-to-install-and-use-kubernetes-on-ubuntu-20-04/)

        kubeadm join 192.168.15.201:6443 --token zz9ji1.ng8gtkjmtunqw2s6 --discovery-token-ca-cert-hash sha256:4c942b3d40b788f85aa71d020a59529d23498326744d09d3835f8fd799a06311

