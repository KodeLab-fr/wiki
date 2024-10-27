# K8S

## Init a k8s with kubeadm

Go take a look at [kubeadm](../../tools/cli/kubeadm.md) it is the tools that we are going to use for this setup.
Here is my default config
```yaml
---
kind: ClusterConfiguration
apiVersion: kubeadm.k8s.io/v1beta4
kubernetesVersion: v1.31.0
networking:
  podSubnet: "192.168.0.0/16"
---
kind: KubeletConfiguration
apiVersion: kubelet.config.k8s.io/v1beta1
cgroupDriver: cgroupfs
---
kind: InitConfiguration
apiVersion: kubeadm.k8s.io/v1beta4
nodeRegistration:
  criSocket: unix:///var/run/containerd/containerd.sock
  taints: []
```

And to init the cluster
```bash
sudo kubeadm init --config /etc/kubernetes/kube-config.yaml
```


## Networking

(We are gonna use calico but you can use flannel or anything you want it is gonna be similar)
Now we are ready to setup our network
Here are the two command line to setup calico:
```bash
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.2/manifests/tigera-operator.yaml
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.2/manifests/custom-resources.yaml
```

## Storage 

(We are going to use longhorn but take a look at rook ect)
You need to take a look at the documentation https://longhorn.io/docs/1.7.2/deploy/install/

First install the requirements:
```bash
sudo apt install open-iscsi
sudo systemctl enable --now iscsid
```

```bash
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.7.2/deploy/longhorn.yaml
```

