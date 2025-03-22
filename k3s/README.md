# K3s on Jetson


## Prerequisites
- NVIDIA Device Driver
- NVIDIA Container Toolkit setup for containerd/docker


## Install k3s

Install with nvidia runtime as default, it will use containerd
```shell
curl -sfL https://get.k3s.io | sh -s - --default-runtime nvidia --disable=traefik
```

k3s script will detect nvidia container-toolkit runtime
```shell
grep nvidia /var/lib/rancher/k3s/agent/etc/containerd/config.toml
```

Use kubectl
```shell
sudo kubectl get pods -A --kubeconfig /etc/rancher/k3s/k3s.yaml
```

## Install Device Plugin

Label the node with a gpu
```shell
kubectl label node jetson nvidia.com/gpu.present=true
```

Add helm repo
```shell
helm repo add nvdp https://nvidia.github.io/k8s-device-plugin
helm repo update
```
Deploy with feature discovery (--set gfd.enabled=true)
```shell
helm upgrade -i nvdp nvdp/nvidia-device-plugin \
  --namespace nvidia-device-plugin \
  --create-namespace \
  --version 0.17.1 \
  --set gfd.enabled=true
```

CUDA Time-slicing
```shell
cat << EOF > nvidia-device-plugin.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: nvidia-device-plugin
  namespace: nvidia-device-plugin
data:
  config0: |-
    version: v1
    flags:
      migStrategy: none
    sharing:
      timeSlicing:
        resources:
        - name: nvidia.com/gpu
          replicas: 4
EOF
kubectl apply -f nvidia-device-plugin.yaml
```

Redeploy
```shell
helm upgrade -i nvdp nvdp/nvidia-device-plugin \
  --namespace nvidia-device-plugin \
  --create-namespace \
  --version 0.17.1 \
  --set gfd.enabled=true \
  --set config.default=config0 \
  --set config.name=nvidia-device-plugin
```

Prompt
```
write a 2 page document that states why is kubernetes is used so much for platform engineering
```

## Uninstall
```shell
/usr/local/bin/k3s-uninstall.sh
```

### References:
- [NVIDIA Device Plugin](https://github.com/NVIDIA/k8s-device-plugin)
- [K3S installation on Jetson AGX Orin with Jetpack 6](https://forums.developer.nvidia.com/t/k3s-installation-on-jetson-agx-orin-with-jetpack-6/283213/20)
- [Config](https://docs.k3s.io/installation/configuration)
- [Containerd](https://docs.k3s.io/advanced?_highlight=containerd#configuring-containerd)
- [CNI](https://docs.k3s.io/networking/basic-network-options)


### Appendix

Logs for Device Plugin
```
I0322 16:43:11.561301       1 main.go:235] "Starting NVIDIA Device Plugin" version=<
        3c378193
        commit: 3c378193fcebf6e955f0d65bd6f2aeed099ad8ea
 >
I0322 16:43:11.561465       1 main.go:238] Starting FS watcher for /var/lib/kubelet/device-plugins
I0322 16:43:11.561618       1 main.go:245] Starting OS watcher.
I0322 16:43:11.561988       1 main.go:260] Starting Plugins.
I0322 16:43:11.562011       1 main.go:317] Loading configuration.
I0322 16:43:11.564265       1 main.go:342] Updating config with default resource matching patterns.
I0322 16:43:11.564638       1 main.go:353]
Running with config:
{
  "version": "v1",
  "flags": {
    "migStrategy": "none",
    "failOnInitError": true,
    "mpsRoot": "/run/nvidia/mps",
    "nvidiaDriverRoot": "/",
    "nvidiaDevRoot": "/",
    "gdsEnabled": false,
    "mofedEnabled": false,
    "useNodeFeatureAPI": null,
    "deviceDiscoveryStrategy": "auto",
    "plugin": {
      "passDeviceSpecs": false,
      "deviceListStrategy": [
        "envvar"
      ],
      "deviceIDStrategy": "uuid",
      "cdiAnnotationPrefix": "cdi.k8s.io/",
      "nvidiaCTKPath": "/usr/bin/nvidia-ctk",
      "containerDriverRoot": "/driver-root"
    }
  },
  "resources": {
    "gpus": [
      {
        "pattern": "*",
        "name": "nvidia.com/gpu"
      }
    ]
  },
  "sharing": {
    "timeSlicing": {}
  },
  "imex": {}
}
I0322 16:43:11.564660       1 main.go:356] Retrieving plugins.
I0322 16:43:11.583524       1 server.go:195] Starting GRPC server for 'nvidia.com/gpu'
I0322 16:43:11.584898       1 server.go:139] Starting to serve 'nvidia.com/gpu' on /var/lib/kubelet/device-plugins/nvidia-gpu.sock
I0322 16:43:11.591052       1 server.go:146] Registered device plugin for 'nvidia.com/gpu' with Kubelet
```