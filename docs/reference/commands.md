# Useful commands

## Slurm essentials

```bash
squeue # see running/queued jobs
sbatch script.sh # submit job
salloc --mem=10G --cpus-per-task=4 --time=01:00:00 # reserve interactive resources
scancel <JOBID> # cancel job
```

Node details:

```bash
scontrol show node <node> # see node description
```

Spack essentials:

```bash
spack search <name>
spack install <pkg>
spack load <pkg>
spack find
spack unload <pkg>
```

Conda essentials:

```bash
conda create -n myenv python=3.11
conda activate myenv
conda deactivate
```
