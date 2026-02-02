# Cluster resources

Approximate available resources:

- ~192 vCPUs
- ~250GB RAM
- 2 × RTX 6000 Ada (48GB VRAM each)

## Memory defaults

By default, Slurm jobs are assigned **1GB RAM per CPU core** unless you request more.

## Checking availability of resources

```bash
winfo
```
