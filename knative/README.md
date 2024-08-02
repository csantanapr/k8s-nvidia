# Knative on Jetson


Setup docker and containerd default nvidia runtime using [jetson-containers](../jetson-containers.md)

Edit `/etc/nvidia-container-runtime/config.toml`
Set at the top of the file `accept-nvidia-visible-devices-as-volume-mounts = true`

```bash
# Create kind cluster name knative
kind create cluster --name knative --config ../kind/kind.yaml
```

```bash
# Install knative using kn
kn quickstart kind
# Answer N
# Delete and recreate [y/N]: N
```

```bash
# Enable pvc and write access
kubectl apply -f knative-features.yaml
```

```bash
# Create pv and pvc
kubectl apply -f ollama-pvc.yaml
```


```bash
# Delete cluster
kind delete cluster --name knative
```