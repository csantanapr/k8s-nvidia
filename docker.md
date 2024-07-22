
## Docker
Following instructions from https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04

```bash
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=arm64] https://download.docker.com/linux/ubuntu focal stable"
apt-cache policy docker-ce
sudo apt install docker-ce -y
sudo systemctl status docker
```

Add the ability to run `docker` whithout `sudo
```bash
sudo usermod -aG docker ${USER}
su - ${USER}
```

### CUDA Toolkit for Docker and Containerd

Follow instructions here https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#configuring-docker


### CUDA Toolkit Docker
Check the config before:
```bash
cat /etc/docker/daemon.json
```
Add CUDA runtime
```bash
sudo nvidia-ctk runtime configure --runtime=docker
```
Restart docker
```bash
sudo systemctl restart docker
```
Check config after:
```bash
cat /etc/docker/daemon.json
```

### CUDA Toolkit for Containerd
Check the config before:
```bash
cat /etc/containerd/config.toml
```
expected output:
```
disabled_plugins = ["cri"]

#root = "/var/lib/containerd"
#state = "/run/containerd"
#subreaper = true
#oom_score = 0

#[grpc]
#  address = "/run/containerd/containerd.sock"
#  uid = 0
#  gid = 0

#[debug]
#  address = "/run/containerd/debug.sock"
#  uid = 0
#  gid = 0
#  level = "info"
```
Add CUDA runtime
```bash
sudo nvidia-ctk runtime configure --runtime=containerd
```
Restart docker
```bash
sudo systemctl restart containerd
```
Check config after:
```bash
cat /etc/containerd/config.toml
```
expected output:
```
disabled_plugins = ["cri"]
version = 1

[plugins]

  [plugins.cri]

    [plugins.cri.containerd]

      [plugins.cri.containerd.runtimes]

        [plugins.cri.containerd.runtimes.nvidia]
          privileged_without_host_devices = false
          runtime_engine = ""
          runtime_root = ""
          runtime_type = "io.containerd.runc.v2"

          [plugins.cri.containerd.runtimes.nvidia.options]
            BinaryName = "/usr/bin/nvidia-container-runtime"
            Runtime = "/usr/bin/nvidia-container-runtime"
```

## Sample Workload
Ref: https://www.reddit.com/r/docker/comments/unlqlr/use_cuda_within_a_docker_container/

Download the runtime CUDA container:
```bash
docker pull nvidia/cuda:12.2.2-runtime-ubuntu20.04
```

Run and bash into it
```bash
docker run --rm --runtime=nvidia --gpus all -it nvidia/cuda:12.2.2-runtime-ubuntu20.04 bash
```

Run the following code
```bash

apt update
apt install wget -y

# install miniconda
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh
bash Miniconda3-latest-Linux-aarch64.sh -b -p /opt/miniconda

/opt/miniconda/bin/conda init

exec bash
conda create -n pytorch
conda activate pytorch

conda install pytorch torchvision torchaudio cudatoolkit -c pytorch
conda install nvidia/label/cuda-12.2.2::cuda-toolkit -c pytorch

# check if cuda is available
python -m torch.utils.collect_env
```