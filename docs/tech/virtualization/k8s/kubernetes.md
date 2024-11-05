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

## Creation of an Ingress Controller

We are gonna be using nginx but choose the one you want.
Attention the nodePort must be higher than 30000
```bash
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace \
  --set controller.service.type=NodePort \
  --set controller.service.nodePorts.http=30080
```

Now it is time to test everything.
Here is a simple deployement file (named here nginx-example-deployement)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: nginx-example
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

And here a simple (nginx-example-service)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: nginx-example
spec:
  type: ClusterIP
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
```

We now want to show our service to the ingress controller (nginx-example-networking)

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
  namespace: nginx-example
spec:
  ingressClassName: nginx
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: nginx-service
            port:
              number: 80
```

Now it is time to run everything
```bash
kubectl create namespace nginx-example
kubectl apply -f nginx-example-deployement.yaml
kubectl apply -f nginx-example-service.yaml
kubectl apply -f nginx-example-networking.yaml
```

Now you can look at domain.com:30080 or localhost:30080 if you are running locally



### Setup for kind

Setup kind for reverse proxy

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s
```

```bash
kubectl create namespace nginx-example
kubectl apply -f nginx-example-deployement.yaml
kubectl apply -f nginx-example-service.yaml
kubectl apply -f nginx-example-networking.yaml
```
