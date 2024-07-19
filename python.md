# Python resources

Version Managers
Choosing the best Python version manager often depends on your specific needs and preferences. Here are a few popular options:

1. **pyenv**: This is a widely used Python version management tool that allows you to install, manage, and switch between multiple versions of Python. It's flexible and works well across different operating systems.

2. **conda**: While primarily known as a package manager for data science-related libraries, Conda can also manage Python versions effectively through environments. It's especially useful if you work with scientific computing or need to manage dependencies alongside Python versions.

3. **pipenv**: More focused on managing dependencies and virtual environments rather than Python versions specifically, pipenv can also handle Python installations and version switching.

4. **virtualenv** and **venv**: These are built-in Python tools for creating isolated Python environments. While they don't manage multiple Python versions directly, they are commonly used alongside tools like pyenv or directly for simpler needs.

The choice ultimately depends on whether you need to manage multiple Python versions, dependencies, or both, and which tool integrates best with your workflow.


## Conda
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh
bash Miniconda3-latest-Linux-aarch64.sh
```

conda config --set auto_activate_base false

You can undo this by running `conda init --reverse $SHELL`? [yes|no]

You have chosen to not have conda modify your shell scripts at all.
To activate conda's base environment in your current shell session:

eval "$(/home/ubuntu/miniconda3/bin/conda shell.YOUR_SHELL_NAME hook)"

To install conda's shell functions for easier access, first activate, then:
```bash
conda init
```

To create an environment
```bash
conda create --name myenv python=3.11
```
To activate this environment, use
```bash
conda activate myenv
```
To deactivate an active environment, use
```bash
conda deactivate
```