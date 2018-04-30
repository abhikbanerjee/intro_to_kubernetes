# Install and configure the Kubelet

## node1

```
gcloud compute ssh node1
```

### Download Kubernetes release tar

```
sudo snap install kubectl --classic
conjure-up kubernetes
```

### Create the kubelet systemd unit file:

```
curl -O https://storage.googleapis.com/configs.kuar.io/kubelet.service
```

Edit the path to the kubelet. Change

```
ExecStart=/usr/bin/kubelet \
```

to:

```
ExecStart=/opt/bin/kubelet \
```

Configure the api-servers flag:

```
PROJECT_ID=$(curl -H "Metadata-Flavor: Google" \
  http://metadata.google.internal/computeMetadata/v1/project/project-id)
```

```
sed -i -e "s/PROJECT_ID/${PROJECT_ID}/g;" kubelet.service
```

Review the kubelet unit file:

```
cat kubelet.service
```

Start the kubelet service:

```
sudo mv kubelet.service /etc/systemd/system/
```

```
sudo systemctl daemon-reload
sudo systemctl enable kubelet
sudo systemctl start kubelet
```

### Verify

```
sudo systemctl status kubelet
```

#### laptop

```
kubectl get nodes
```
