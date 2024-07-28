# OLLAMA on Raspberry pi
https://ollama.com/

Warning: To get ollama working on Jetson follow instructions here https://forums.developer.nvidia.com/t/ollama-and-jetson-issue/278708/12


## Install Ollama

```bash
curl -fsSL https://ollama.com/install.sh | sh
```
expected output:
```bash
>>> Downloading ollama...
######################################################################## 100.0%##O#-#
>>> Installing ollama to /usr/local/bin...
[sudo] password for csantana:
>>> Creating ollama user...
>>> Adding ollama user to render group...
>>> Adding ollama user to video group...
>>> Adding current user to ollama group...
>>> Creating ollama systemd service...
>>> Enabling and starting ollama service...
Created symlink /etc/systemd/system/default.target.wants/ollama.service → /etc/systemd/system/ollama.service.
>>> NVIDIA GPU installed.
```

GPU with CUDA 12 doesn't work, need to force cpu
```bash
export OLLAMA_LLM_LIBRARY="cpu"
```

## Hello Tinyllama

```bash
ollama list
```

Try a model like `tinyllama` or `llama3-groq-tool-use:8b`
```bash
ollama run tinyllama --verbose
```

Exit with `/bye`

Check the ollama logs
```bash
journalctl -e -u ollama
```
Expected output:
```bash
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: freq_base_train  = 500000.0
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: freq_scale_train = 1
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: n_ctx_orig_yarn  = 8192
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: rope_finetuned   = unknown
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: ssm_d_conv       = 0
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: ssm_d_inner      = 0
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: ssm_d_state      = 0
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: ssm_dt_rank      = 0
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: model type       = 8B
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: model ftype      = Q4_0
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: model params     = 8.03 B
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: model size       = 4.33 GiB (4.64 BPW)
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: general.name     = Llama-3-Groq-8B-Tool-Use
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: BOS token        = 128000 '<|begin_of_text|>'
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: EOS token        = 128009 '<|eot_id|>'
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: PAD token        = 128009 '<|eot_id|>'
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: LF token         = 128 'Ä'
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: EOT token        = 128009 '<|eot_id|>'
Jul 28 14:26:49 jetson ollama[202633]: llm_load_print_meta: max token length = 256
Jul 28 14:26:49 jetson ollama[202633]: ggml_cuda_init: GGML_CUDA_FORCE_MMQ:    no
Jul 28 14:26:49 jetson ollama[202633]: ggml_cuda_init: GGML_CUDA_FORCE_CUBLAS: no
Jul 28 14:26:49 jetson ollama[202633]: ggml_cuda_init: found 1 CUDA devices:
Jul 28 14:26:49 jetson ollama[202633]:   Device 0: Orin, compute capability 8.7, VMM: yes
Jul 28 14:31:48 jetson ollama[202633]: time=2024-07-28T14:31:48.554-04:00 level=ERROR source=sched.go:443 msg="error loading llama server" error="timed out waiting for llama runner to start - progress 0.00 - "
Jul 28 14:31:48 jetson ollama[202633]: [GIN] 2024/07/28 - 14:31:48 | 500 |          5m0s |       127.0.0.1 | POST     "/api/chat"
Jul 28 14:31:53 jetson ollama[202633]: time=2024-07-28T14:31:53.677-04:00 level=WARN source=sched.go:634 msg="gpu VRAM usage didn't recover within timeout" seconds=5.122682092 model=/usr/share/ollama/.ollama/models/blobs/sha256-23e78d0ea1ab895bcbe63a12422a731cfd2b1a8bd124a1c91790b2b9c3fc9d14
Jul 28 14:31:53 jetson ollama[202633]: time=2024-07-28T14:31:53.928-04:00 level=WARN source=sched.go:634 msg="gpu VRAM usage didn't recover within timeout" seconds=5.373259641 model=/usr/share/ollama/.ollama/models/blobs/sha256-23e78d0ea1ab895bcbe63a12422a731cfd2b1a8bd124a1c91790b2b9c3fc9d14
Jul 28 14:31:54 jetson ollama[202633]: time=2024-07-28T14:31:54.178-04:00 level=WARN source=sched.go:634 msg="gpu VRAM usage didn't recover within timeout" seconds=5.623454524 model=/usr/share/ollama/.ollama/models/blobs/sha256-23e78d0ea1ab895bcbe63a12422a731cfd2b1a8bd124a1c91790b2b9c3fc9d14
Jul 28 14:32:03 jetson ollama[202633]: [GIN] 2024/07/28 - 14:32:03 | 200 |      54.305µs |       127.0.0.1 | HEAD     "/"
Jul 28 14:32:03 jetson ollama[202633]: [GIN] 2024/07/28 - 14:32:03 | 404 |     178.725µs |       127.0.0.1 | POST     "/api/show"
Jul 28 14:32:04 jetson ollama[202633]: time=2024-07-28T14:32:04.894-04:00 level=INFO source=download.go:136 msg="downloading 2af3b81862c6 in 7 100 MB part(s)"
```

Using tinyllama
```
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: freq_scale_train = 1
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: n_ctx_orig_yarn  = 2048
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: rope_finetuned   = unknown
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: ssm_d_conv       = 0
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: ssm_d_inner      = 0
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: ssm_d_state      = 0
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: ssm_dt_rank      = 0
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: model type       = 1B
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: model ftype      = Q4_0
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: model params     = 1.10 B
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: model size       = 606.53 MiB (4.63 BPW)
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: general.name     = TinyLlama
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: BOS token        = 1 '<s>'
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: EOS token        = 2 '</s>'
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: UNK token        = 0 '<unk>'
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: PAD token        = 2 '</s>'
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: LF token         = 13 '<0x0A>'
Jul 28 14:32:21 jetson ollama[202633]: llm_load_print_meta: max token length = 48
Jul 28 14:32:21 jetson ollama[202633]: ggml_cuda_init: GGML_CUDA_FORCE_MMQ:    no
Jul 28 14:32:21 jetson ollama[202633]: ggml_cuda_init: GGML_CUDA_FORCE_CUBLAS: no
Jul 28 14:32:21 jetson ollama[202633]: ggml_cuda_init: found 1 CUDA devices:
Jul 28 14:32:21 jetson ollama[202633]:   Device 0: Orin, compute capability 8.7, VMM: yes
Jul 28 14:32:21 jetson ollama[202633]: time=2024-07-28T14:32:21.344-04:00 level=INFO source=server.go:617 msg="waiting for server to become available" status="llm server loading model"
```


Lets do vision
```
ollama run llava
```
