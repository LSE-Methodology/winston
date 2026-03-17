# Policies and good practice

## Fair usage

- Do not run intensive workloads on the login node (small tests are fine).
- Use Slurm for anything requiring >2 cores or >4GB RAM.
- Prefer short interactive sessions for debugging; submit long runs as batch jobs.

## Storage

Your home directory lives on the **root drive**, which is shared with the OS. Keeping it lean is important for system stability.

- Store **code, configs, and environments** in `/home`.
- Store **large datasets, model outputs, and caches** in `/scratch`.
- Use **`/data/shared/`** for datasets multiple users need.
- See **Reference → Cluster resources** for quotas and directory details.

!!! warning "Avoid filling your home directory"
    Large Slurm log files, HuggingFace model caches, and forgotten data copies are the most common causes of home directory bloat.

### Direct Slurm output to scratch

By default, Slurm writes `.out` and `.err` files to the directory you submitted from. Always direct output to scratch:

```bash
#SBATCH --output=/scratch/%u/%x_%j.out
#SBATCH --error=/scratch/%u/%x_%j.err
```

`%u` = username, `%x` = job name, `%j` = job ID.

### Symlink large caches to scratch

If you use HuggingFace, Ollama, or other tools that cache large model files, move the cache to scratch and symlink:

```bash
# Move existing cache
mv ~/.cache /scratch/$USER/cache
ln -s /scratch/$USER/cache ~/.cache

# Or set environment variables (add to ~/.bashrc)
export HF_HOME=/scratch/$USER/hf_cache
export TRANSFORMERS_CACHE=/scratch/$USER/hf_cache
```

### Keep conda environments lean

Conda environments can grow to tens of GB. Periodically:

1. Export environment specs so they can be recreated: `conda env export > environment.yml`
2. Clean unused packages: `conda clean --all`
3. Remove environments you no longer use: `conda env remove -n <name>`

## Software installs

- Keep Spack builds under `/data/spack/users/$USER`.
- Do not modify anything under `/opt/spack`.
- Remove unused Conda environments occasionally.
- Export environments for reproducibility:
  - `conda env export > env.yml`
  - `spack find --format '{name}@{version}' > spack-env.txt`

## Time limits

- Always set `#SBATCH --time` realistically.
- Jobs exceeding walltime will be terminated.
