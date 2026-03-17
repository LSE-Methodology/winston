# Useful commands

## Slurm essentials

```bash
squeue # see running/queued jobs
squeue -u $USER # see your jobs
sbatch script.sh # submit job
salloc --mem=10G --cpus-per-task=4 --time=01:00:00 # reserve interactive resources
scancel <JOBID> # cancel job
sinfo # view partitions / nodes
sacct -j <JOBID> # view job history
```

Node details:

```bash
scontrol show node <node> # see node description
scontrol show job <JOBID> # see job details
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

## Storage & quotas

```bash
du -sh ~/*/ 2>/dev/null | sort -rh | head -10    # home directory breakdown
du -sh ~/.[^.]* 2>/dev/null | sort -rh | head -5  # hidden directories (often large)
du -sh /scratch/$USER                              # scratch usage
quota -s                                           # check your quota status
```
