# Set up before you start

## Managing python environment

### Create environment

I recommend using Anaconda to manage your python environment, so that any packages install in this project (e.g. scikit-learn) does not interfere with others. To install Anaconda, go to [anaconda](https://www.anaconda.com/docs/getting-started/anaconda/install).

Once anaconda install, create a project-specific python environment by typing in the terminal:

```
# bioinfor-to-ai-env is the name of the environment
# specify using python3.10
conda create -n bioinfor-to-ai-env python=3.10
```

Activate the environment everytime before you start working on the project:

```
conda activate bioinfor-to-ai-env
```

Switch to conda base environment when you finish working:

```
deactivate
```

### Install packages
After your environment is activated, you can install the packages used in this project by:

```
# I only listed the essential packages
pip install -r requirements.txt
```
or if you use conda:
```
# 1. Create a clean environment
conda create -n cat-ai python=3.10 -y
conda activate cat-ai

# 2. Install PyTorch (The core engine)
# For CPU:
conda install pytorch torchvision cpuonly -c pytorch
# For NVIDIA GPU:
# conda install pytorch torchvision pytorch-cuda=11.8 -c pytorch -c nvidia

# 3. Install Data Science stack
pip install matplotlib scikit-learn seaborn pandas ipykernel
```


## Productivity

### Documentation

I use the Markdown format to write this document and all the README and .ipynb in this project. I strongly recommend learning it for clear documentation:

* A good place to start: [learn-markdown](https://learn-markdown.github.io/)
* Quick reference: [Markdown-cheat-sheet](https://www.markdownguide.org/cheat-sheet/)

### IDE
I recommend using [Visual Studio Code](https://code.visualstudio.com/). It supports all operation systems (Windows, Linux, MacOS), has integrated AI code editor (GitHub Copilot etc), and works great with jupyter notebooks. And it's free!

### Tracking Changes

I use git to track changes I made in this project including adding/editing/deleting files. A few links:

* [install-git](https://github.com/git-guides/install-git)
* [git-basic-commends](https://github.com/git-guides#learning-git-basics): these should be sufficient for standalone project like this

I strongly recommend having a `.gitignore` file at your project root like this: [.gitignore](../.gitignore). It tells git not to track files that are either too large (e.g. the gene expression data I downloaded) or files containing sensitive information, such as your password.
