# Installing Kubernetes

- [Install JetPack 6](./jetson-setup.md)
- [Setup docker and containerd](./docker.md)
- Disable swap since it may cause issue with Kubernetes. Please notes that swap will be activated everytime the system starts. Remember to disable it.
```bash
sudo swapoff -a
```
- Install kubeadm
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

```bash
sudo apt-get update
# apt-transport-https may be a dummy package; if so, you can skip that package
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
```

```bash
# If the directory `/etc/apt/keyrings` does not exist, it should be created before the curl command, read the note below.
# sudo mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

```bash
# This overwrites any existing configuration in /etc/apt/sources.list.d/kubernetes.list
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

```bash
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

Start kubelet
```bash
sudo systemctl enable --now kubelet
```

Edit containerd and make sure SystemdCgroup is `true`
```
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
  SystemdCgroup = true
```

```bash
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```



```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Apply flannel to the cluster
```bash
sudo sysctl net.bridge.bridge-nf-call-iptables=1
curl -O https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
sudo kubectl apply -f kube-flannel.yml
```

remove taint
```bash
kubectl taint nodes jetson node-role.kubernetes.io/control-plane:NoSchedule-
```

aliases
```bash
alias log-kubelet="sudo journalctl -u kubelet -f"
alias log-containerd="sudo journalctl -u containerd -f"
```


remove label
```bash
kubectl label node jetson node.kubernetes.io/exclude-from-external-load-balancers-
```

```bash
helm repo add metallb https://metallb.github.io/metallb
helm install metallb metallb/metallb --namespace metallb-system --create-namespace
```

```bash
kubectl metal-lb-pool.yaml
```


References:
- https://forums.developer.nvidia.com/t/jetpack-6-3-containerd-and-kubernetes/296483/13
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
- https://github.com/vincent51689453/Kubernetes-Jetson-GPU-Clusters/tree/master

### C. Common Issue
1) ***ERROR "no such files -> /run/flannel/subnet.env"***
Solu: copy this subnet.env file from master to all slaves which do not have this file

2) ***Error "cni0" already has an IP address different from 10.244.1.1/24***
Solu:
**Inside Slave**
```
$ sudo ifconfig  cni0 down
$ sudo brctl delbr cni0
$ sudo ip link delete flannel.1
$ kubeadm reset
$ # run the kubeadm init  or join

```


3) ***DNS Issue: you can ping through ip address but not domain name. You are failed to perform apt-get update inside container***
```
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf > /dev/null
where 8.8.8.8 should be 158.132.14.1 in my case