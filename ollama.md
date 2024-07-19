# OLLAMA on Raspberry pi
https://ollama.com/

For setting up python environment see [python.md](python.md)
youtube [Using Ollama to Run Local LLMs on the Raspberry Pi 5](https://www.youtube.com/watch?v=ewXANEIC8pY)

## Install Ollama

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

## Hello Tinyllama

```bash
ollama list
```

```bash
ollama run tinyllama --verbose
```

Exit with `/bye`

Try llama2
```bash
ollama run llama2-uncensored --verbose
```

Lets do vision
```
ollama run llava
```
