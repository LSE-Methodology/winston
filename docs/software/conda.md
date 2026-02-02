# Miniconda

Miniconda is recommended for user-level environments (Python, R packages, data-science tools).

## First-time install

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh -b -p $HOME/miniconda3
source ~/miniconda3/etc/profile.d/conda.sh
conda config --set auto_activate_base false
conda config --add channels conda-forge
conda config --set channel_priority strict
```

## Shell setup

Add to `~/.bashrc`:

```bash
if [ -f "$HOME/miniconda3/etc/profile.d/conda.sh" ]; then
    source "$HOME/miniconda3/etc/profile.d/conda.sh"
fi
```

Reload your bash profile:

```bash
source ~/.bashrc
```

Create and use an environment:

```bash
conda create -n py311 python=3.11 -y
conda activate py311
conda install numpy pandas matplotlib
```

Deactivate:

```bash
conda deactivate
Reproducibility
conda env export > env.yml
```
