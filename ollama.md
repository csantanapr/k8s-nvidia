# OLLAMA
https://ollama.com/

Install [jetson-containers](./jetson-containers.md)

Follow instructions from https://github.com/dusty-nv/jetson-containers/tree/master/packages/llm/ollama

```bash
jetson-containers run --name ollama $(autotag ollama)
```

```bash
# if running inside the same container as launched above
/bin/ollama run mistral

# if launching a new container for the client in another terminal
jetson-containers run $(autotag ollama) /bin/ollama run mistral
```


```bash
# Running without host network
docker run -it --rm --volume /home/csantana/jetson-containers/data:/data -p 11434:11434 dustynv/ollama:r36.2.0
```

docker run --rm --volume /home/csantana/jetson-containers/data:/data -p 11434:11434 dustynv/ollama:r36.2.0 ollama serve

curl ollama.default.127.0.0.1.sslip.io/api/generate -d '{
  "model": "llama3-groq-tool-use:8b",
  "prompt":"generate terraform to create eks cluster with everything"
}'

docker run -it --rm --network=host --add-host=host.docker.internal:host-gateway ghcr.io/open-webui/open-webui:main


## Hello Tinyllama

```bash
ollama list
```

Try a model like `tinyllama` or `llama3-groq-tool-use:8b`
```bash
ollama run tinyllama --verbose
```

Exit with `/bye`


Lets do vision
```
ollama run llava
```
