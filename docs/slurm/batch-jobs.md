# Batch jobs

Batch jobs are submitted with `sbatch`. You describe what you need (time, memory, CPUs), and Slurm schedules it.

## Example: CPU batch job

Create a script, e.g. `job.sh`:

```bash
#!/bin/bash
#SBATCH --job-name=my_python_job
#SBATCH --output=/scratch/%u/%x_%j.out
#SBATCH --error=/scratch/%u/%x_%j.err
#SBATCH --time=01:00:00        # Max walltime (hh:mm:ss)
#SBATCH --mem=10G              # Memory per job
#SBATCH --cpus-per-task=4      # Number of CPU cores
#SBATCH --partition=winston    # Default partition (optional)

python my_script.py
```

Submit it:

```bash
sbatch job.sh
```

Check status:

```bash
squeue
```

!!! warning "Walltime default"
    If you do not set --time, the current default is 12 hours.
    Jobs that exceed their walltime will be terminated by the scheduler.

!!! note "Memory requests"
    `--mem` is total memory for the job. If you prefer per-core memory, use `--mem-per-cpu`.

!!! tip "Direct output to scratch"
    Always write Slurm output to `/scratch/%u/` instead of your home directory. Large log files can fill your home directory unexpectedly.
    `%u` = your username, `%x` = job name, `%j` = job ID.

## Per-job scratch directory

Every Slurm job automatically gets a private temporary directory at `/scratch/$USER/job_$SLURM_JOB_ID`. It is created before the job starts and **deleted when the job ends**. Use it for throwaway intermediate files.

```bash
#!/bin/bash
#SBATCH --job-name=my_job
#SBATCH --output=/scratch/%u/job_%j.out

JOB_SCRATCH="/scratch/$USER/job_$SLURM_JOB_ID"

# Copy input data to fast per-job scratch
cp /data/shared/my_dataset.csv "$JOB_SCRATCH/"
cd "$JOB_SCRATCH"

# ... run your computation ...

# Copy results to persistent storage before the job ends
cp results.csv /scratch/$USER/my_project/
```

!!! warning
    Files left only in the per-job scratch directory are lost when the job finishes. Always copy final results to your persistent scratch or home directory.
