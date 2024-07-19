# LLAMA.CPP on Raspberry pi

For setting up python environment see [python.md](python.md)

## LLAMA CPP
https://github.com/ggerganov/llama.cpp

You need to be able to compile llama.cpp

Install Make and CMake
```bash
sudo apt install make -y
sudo apt install cmake -y
sudo apt install build-essential -y
sudo apt install ccache -y
sudo apt-get install libcurl4-openssl-dev
```
Clone the repo
```bash
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
```
Build with libcurl support:
```bash
LLAMA_CURL=1 make
```

# Load and run the model from [Hugginface](https://huggingface.co/)
Run it with a prompt:
```bash
./llama-cli \
	--hf-repo "TheBloke/Llama-2-7B-Chat-GGUF" \
  --hf-file "llama-2-7b-chat.Q4_K_M.gguf" \
	-m "models/llama-2-7b-chat.Q4_K_M.gguf" \
  -p "Generate a CLI python script to get the current time from top ten most pupular timezones, printed in a table in terminal output"
```
Run in in interactive mode:
```bash
./llama-cli \
	--hf-repo "TheBloke/CodeLlama-34B-Python-GGUF" \
  --hf-file "codellama-34b-python.Q5_0.gguf" \
	-m "models/codellama-34b-python.Q5_0.gguf" \
  -i
```



# llama cpp python bindings
https://github.com/abetlen/llama-cpp-python

Requirements:
Python 3.8+
C compiler Linux: gcc or clang

Pre-built Wheel (New)
It is also possible to install a pre-built wheel with basic CPU support.
```bash
pip install llama-cpp-python \
  --extra-index-url https://abetlen.github.io/llama-cpp-python/whl/cpu
```

# Easy llama
https://github.com/ddh0/easy-llama

easy-llama's purpose is to make use of on-device large language models (LLMs) as easily as possible. It is a layer of abstraction over llama-cpp-python, which itself provides the Python bindings for the underlying llama.cpp project.

Install CPU only option:
```bash
pip uninstall llama-cpp-python -y
pip install --no-cache-dir llama-cpp-python
pip install --upgrade --no-cache-dir easy-llama
```

```
import easy_llama as ez
Mixtral = ez.Model('Mixtral-8x7B-v0.1-q8_0.gguf')
Mixtral.generate('The sky is blue, and')
' the grass is green. It seems like the most natural thing in the world to most of us. However, have you ever stopped to think that the color of these things is actually a perception of our brain?'
```