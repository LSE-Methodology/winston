# Batch jobs

Batch jobs are submitted with `sbatch`. You describe what you need (time, memory, CPUs), and Slurm schedules it.

## Example: CPU batch job

Create a script, e.g. `job.sh`:

```bash
#!/bin/bash
#SBATCH --job-name=my_python_job
#SBATCH --output=output_%j.log
#SBATCH --error=error_%j.log
#SBATCH --time=01:00:00        # Max walltime (hh:mm:ss)
#SBATCH --mem=10G              # Memory per job
#SBATCH --cpus-per-task=4      # Number of CPU cores

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
