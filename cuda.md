## CUDA

## App CUDA to PATH
```bash
echo "export PATH=\$PATH:/usr/local/cuda/bin" >> $HOME/.bashrc
source $HOME/.bashrc
```

## Check version
```bash
nvcc --version
```
expected output:
```
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2023 NVIDIA Corporation
Built on Tue_Aug_15_22:08:11_PDT_2023
Cuda compilation tools, release 12.2, V12.2.140
Build cuda_12.2.r12.2/compiler.33191640_0
```

## Upgrade CUDA (Optional)
https://developer.nvidia.com/cuda-downloads


## Query the GPU
```bash
git clone https://github.com/NVIDIA/cuda-samples.git
cd cuda-samples/Samples/1_Utilities/deviceQuery
make
./deviceQuery
```
Expected output:
```
./deviceQuery Starting...

 CUDA Device Query (Runtime API) version (CUDART static linking)

Detected 1 CUDA Capable device(s)

Device 0: "Orin"
  CUDA Driver Version / Runtime Version          12.2 / 12.2
  CUDA Capability Major/Minor version number:    8.7
  Total amount of global memory:                 15657 MBytes (16417153024 bytes)
  (004) Multiprocessors, (128) CUDA Cores/MP:    512 CUDA Cores
  GPU Max Clock rate:                            918 MHz (0.92 GHz)
  Memory Clock rate:                             612 Mhz
  Memory Bus Width:                              256-bit
  L2 Cache Size:                                 2097152 bytes
  Maximum Texture Dimension Size (x,y,z)         1D=(131072), 2D=(131072, 65536), 3D=(16384, 16384, 16384)
  Maximum Layered 1D Texture Size, (num) layers  1D=(32768), 2048 layers
  Maximum Layered 2D Texture Size, (num) layers  2D=(32768, 32768), 2048 layers
  Total amount of constant memory:               65536 bytes
  Total amount of shared memory per block:       49152 bytes
  Total shared memory per multiprocessor:        167936 bytes
  Total number of registers available per block: 65536
  Warp size:                                     32
  Maximum number of threads per multiprocessor:  1536
  Maximum number of threads per block:           1024
  Max dimension size of a thread block (x,y,z): (1024, 1024, 64)
  Max dimension size of a grid size    (x,y,z): (2147483647, 65535, 65535)
  Maximum memory pitch:                          2147483647 bytes
  Texture alignment:                             512 bytes
  Concurrent copy and kernel execution:          Yes with 2 copy engine(s)
  Run time limit on kernels:                     No
  Integrated GPU sharing Host Memory:            Yes
  Support host page-locked memory mapping:       Yes
  Alignment requirement for Surfaces:            Yes
  Device has ECC support:                        Disabled
  Device supports Unified Addressing (UVA):      Yes
  Device supports Managed Memory:                Yes
  Device supports Compute Preemption:            Yes
  Supports Cooperative Kernel Launch:            Yes
  Supports MultiDevice Co-op Kernel Launch:      Yes
  Device PCI Domain ID / Bus ID / location ID:   0 / 0 / 0
  Compute Mode:
     < Default (multiple host threads can use ::cudaSetDevice() with device simultaneously) >

deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 12.2, CUDA Runtime Version = 12.2, NumDevs = 1
Result = PASS
```

conda create --name myenv python=3.10
conda activate myenv


Test pytorch cuda
https://developer.download.nvidia.com/compute/redist/jp/v60/pytorch/

wget https://developer.download.nvidia.com/compute/redist/jp/v60/pytorch/torch-2.4.0a0+07cecf4168.nv24.05.14710581-cp310-cp310-linux_aarch64.whl -O torch-2.4.0a0+07cecf4168.nv24.05.14710581-cp310-cp310-linux_aarch64.whl

pip install torch-2.4.0a0+07cecf4168.nv24.05.14710581-cp310-cp310-linux_aarch64.whl

```python
import torch

print("PyTorch version:", torch.__version__)

# Check if CUDA is available
print("CUDA is available:", torch.cuda.is_available())

# Get the number of available GPUs
print("Number of GPUs:", torch.cuda.device_count())

# Get the current CUDA device
print("Current CUDA device:", torch.cuda.current_device())

# Get the name of the current CUDA device
print("Name of the current CUDA device:", torch.cuda.get_device_name(torch.cuda.current_device()))

```


Fine Tune:
https://wiki.seeedstudio.com/Finetune_LLM_on_Jetson/
