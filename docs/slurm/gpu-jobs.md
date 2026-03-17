# GPU jobs

Use the GPU partition for CUDA workloads.

## Partitions

- `winston` (default): general CPU queue (use for most jobs)
- `winston-gpu`: for GPU jobs (CUDA) and privileged access to GPU resources

!!! note "GPU partition limits"
    The `winston-gpu` partition has **4 dedicated CPUs** and **50GB exclusive RAM**.
    Additional resources may be available depending on cluster load, but are not guaranteed.

## Example: GPU batch job

Create a batch script `my_gpu_job.sh`:

```bash
#!/bin/bash
#SBATCH --job-name=my_gpu_job
#SBATCH --partition=winston-gpu
#SBATCH --gres=gpu:1
#SBATCH --time=02:00:00
#SBATCH --mem=20G
#SBATCH --cpus-per-task=4
#SBATCH --output=/scratch/%u/%x_%j.out
#SBATCH --error=/scratch/%u/%x_%j.err

python train.py
```

Submit with:

```bash
sbatch my_gpu_job.sh
```

!!! tip
    Once running, use `scontrol show job <JOBID>` or `nvidia-smi` (on the node) to confirm GPU allocation.
