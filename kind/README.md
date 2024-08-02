# Kind on Jetson

```bash
# Create kind cluster name knative
kind create cluster --name gpu-test --config ../kind/kind.yaml
```

```bash
# install helm
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 \
    && chmod 700 get_helm.sh \
    && ./get_helm.sh
```

```bash
helm install --wait --generate-name \
     -n gpu-operator --create-namespace \
     nvidia/gpu-operator \
     --set driver.enabled=false
```


https://www.substratus.ai/blog/kind-with-gpus

https://github.com/NVIDIA/k8s-device-plugin/issues/332#issuecomment-1917537232

https://github.com/kubernetes-sigs/kind/issues/3164

https://github.com/klueska/nvkind


https://github.com/NVIDIA/k8s-device-plugin/issues/332#issuecomment-1917537232

https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/latest/getting-started.html#gpu-operator-helm-chart-options

