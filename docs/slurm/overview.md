# Slurm overview

Winston uses **Slurm** to queue jobs and allocate compute resources fairly.

You can use Winston in two main ways:

1. **Batch jobs (preferred):** submit a script; Slurm runs it when resources are available.
2. **Interactive jobs:** reserve resources, then work on a compute node.

!!! warning "Do not run heavy work on the login node"
    Small tests are fine, but intensive workloads must run via Slurm.
    Login nodes are limited to **2 CPU cores and 4GB RAM**.

## Quick commands

```bash
squeue           # view jobs
scancel <JOBID>  # cancel a job
winfo            # view allocated resources / cluster usage (local helper)
```
