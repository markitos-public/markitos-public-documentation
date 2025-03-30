# Configuración Inicial

```bash
sudo su
echo "markitos    ALL = (ALL) NOPASSWD: ALL" >>/etc/sudoers
exit

sudo -l && sleep 2
sudo swapoff -a &&
sudo sed -i 's/^\/swap/#\/swap/g' /etc/fstab &&
sudo apt update && sudo apt upgrade -y && sudo apt install -y ccze software-properties-common nano git sshpass locate curl apt-transport-https ca-certificates curl net-tools locate btop && \
sudo updatedb

sudo su
echo "" >> /etc/hosts
echo "#:{.'.}>----- prometeo k8s nodes -----" >> /etc/hosts
echo "192.168.1.200 prometeo1" >> /etc/hosts
echo "192.168.1.201 prometeo2" >> /etc/hosts
echo "192.168.1.202 prometeo3" >> /etc/hosts
echo "192.168.1.250 titan1" >> /etc/hosts
echo "192.168.1.251 titan2" >> /etc/hosts
echo "192.168.1.252 titan3" >> /etc/hosts
echo "#:{.'.}>----- prometeo k8s nodes -----" >> /etc/hosts
echo "" >> /etc/hosts
exit
```

---

## Configuración de módulos para Kubernetes

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF
sudo modprobe overlay
sudo modprobe br_netfilter
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
sudo echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.conf
sudo sysctl --system
```

---

## Instalación de Docker y configuración de Containerd

```bash
sudo install -m 0755 -d /etc/apt/keyrings && \
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc && \
sudo chmod a+r /etc/apt/keyrings/docker.asc && \
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null && \
sudo apt update && \
sudo apt install containerd.io && \
echo "" && echo "CONTAINERD OK" && echo ""

containerd config default | sudo tee /etc/containerd/config.toml
sudo nano -w /etc/containerd/config.toml # Ajustar la versión y SystemdCgroup a true
```

---

## Instalación de Kubernetes y herramientas adicionales

```bash
sudo curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg  && \
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' |  sudo tee /etc/apt/sources.list.d/kubernetes.list  && \
sudo apt update && \
sudo apt install -y kubelet kubeadm kubectl && \
sudo apt-mark hold kubelet kubeadm kubectl
#: k9s for master
sudo wget https://github.com/derailed/k9s/releases/download/v0.32.4/k9s_Linux_amd64.tar.gz  && \
sudo tar xvfz k9s_Linux_amd64.tar.gz && sudo rm k9s_Linux_amd64.tar.gz && sudo rm README.md && sudo rm LICENSE  && \
sudo mv k9s /usr/local/bin/k9s && sudo chmod +x /usr/local/bin/k9s  && \
echo "" && echo "INSTALLED K8S TOOLS INCLUDED K9S OK" && echo ""
echo REBOOT NOW && sleep 2 && sudo reboot
```
---

## Configuración del Nodo Maestro

```bash
sudo kubeadm init --pod-network-cidr=10.10.0.0/16 --control-plane-endpoint=prometeo1
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/tigera-operator.yaml
curl https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/custom-resources.yaml -o calcio-custom-resources.yaml

# Modificar el CIDR en `calcio-custom-resources.yaml` a `10.10.0.0/16` para coincidir con `kubeadm init --pod-network-cidr`
nano -w calcio-custom-resources.yaml
kubectl apply -f calcio-custom-resources.yaml
```

### Configuración DNS

```bash
#: /etc/resolv.conf search 
#: search default.svc.cluster.local svc.cluster.local cluster.local .
```

---

## Comando para Unir Nodos Workers (ejecutar como sudo)

```bash
# Comando de ejemplo para unir nodos (copy + paste kubeadm init output join commands)
```

---