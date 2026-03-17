# Winston HPC — Storage & Usage Guide

> **Last updated:** 2026-03-17
> **Admin contact:** robins75

---

## System Overview

Winston is a single-node HPC server managed with Slurm, equipped with:

- **192 CPUs** (188 allocatable)
- **250GB RAM**
- **2× NVIDIA RTX 6000 Ada Generation GPUs**
- **2× NVMe SSDs** (details below)

### Storage Hardware

| Drive | Model | Capacity | Mount Point | Purpose |
|---|---|---|---|---|
| Micron 2048GB NVMe | nvme0n1 | 1.9TB | `/` (root) | OS, home directories, persistent project data |
| Samsung 1024GB NVMe | nvme1n1 | 954GB | `/scratch` | Temporary/working data, active job data |

---

## Directory Structure

```
/home/<username>/          Persistent personal storage (code, configs, environments)
/scratch/<username>/       Working data, large files, active job data
/scratch/<username>/job_N/ Auto-created per Slurm job, auto-deleted on completion
/data/shared/              Shared datasets (read/write for all hpc-users)
/data/projects/<name>/     Private or collaborative project directories
```

### /home/<username>/

Your home directory is for **code, configuration files, conda/mamba environments, and small files**. It lives on the root drive, which is shared with the operating system, so keeping it lean is important for system stability.

**What belongs here:**
- Source code and scripts
- Conda/mamba environments
- Configuration files
- Small datasets (under a few GB)

**What does NOT belong here:**
- Large datasets (use `/scratch` or `/data`)
- Model checkpoints and intermediate results (use per-job scratch)
- HuggingFace/Ollama model caches (symlink `.cache` to `/scratch`)
- Large log or error output files (direct Slurm output to `/scratch`)

**Hard cap:** 500GB (safety net to prevent disk-filling incidents — not a target!)

**Usage warnings** are displayed at login when your home directory exceeds 50GB, 100GB, or 200GB.

### /scratch/<username>/

Scratch space is on a **separate physical drive** from the OS. It is designed for large, active working data. If scratch fills up, the OS and other users' home directories are unaffected.

**What belongs here:**
- Large datasets you're actively working with
- Model outputs and intermediate results
- HuggingFace/Ollama cache directories (symlinked from `~/.cache`)
- Anything large and reproducible

**Hard cap:** 400GB per user.

**Stale file notices:** Files untouched for 6+ months will trigger a login reminder asking you to archive or remove them. Nothing is automatically deleted.

### /scratch/<username>/job_<jobid>/

Every Slurm job automatically gets a private temporary directory created before the job starts and **deleted when the job ends**. Use this for throwaway intermediate files that you don't need after the job completes.

To use it in your job scripts:

```bash
#!/bin/bash
#SBATCH --job-name=my_job
#SBATCH --output=/scratch/%u/job_%j.out

JOB_SCRATCH="/scratch/$USER/job_$SLURM_JOB_ID"

# Use JOB_SCRATCH for temporary files
cp /data/shared/my_dataset.csv "$JOB_SCRATCH/"
cd "$JOB_SCRATCH"

# ... run your computation ...

# Copy results to persistent storage before the job ends
cp results.csv /scratch/$USER/my_project/
```

**Important:** Any files left only in the per-job scratch directory will be lost when the job finishes. Always copy final results to your persistent scratch or home directory.

### /data/shared/

A shared directory for datasets that multiple users need access to. All members of the `hpc-users` group can read and write here. New files automatically inherit group ownership.

To request a dataset be added, contact the admin.

### /data/projects/<name>/

Private or collaborative project directories. These live on the root drive and are intended for persistent project data that doesn't belong in a single user's home directory.

To request a project directory, contact the admin with:
- Project name
- Which users need access
- Approximate storage needed

---

## Slurm Partitions

| Partition | Default | GPUs | Max Time | Default Time | QoS Allowed |
|---|---|---|---|---|---|
| `winston` | Yes | 2 (shared) | 7 days | 12 hours | high, low, normal, phd |
| `winston-gpu` | No | 2 (dedicated) | 7 days | 12 hours | all |

### QoS by Role

| Account | QoS | Users |
|---|---|---|
| faculty | high | bruntons, geiecke, hubert, power, robins75, sturgis, tsvetkova |
| phd | phd | calvel, cwang |
| postdoc | low | decourson, deschenaux, dickson, gaskin, he, iyer |

---

## Best Practices

### Directing Slurm Output to Scratch

By default, Slurm writes `.out` and `.err` files to the directory you submitted from. Large error logs can fill your home directory unexpectedly. Always direct output to scratch:

```bash
#SBATCH --output=/scratch/%u/%x_%j.out
#SBATCH --error=/scratch/%u/%x_%j.err
```

Where `%u` is your username, `%x` is the job name, and `%j` is the job ID.

### Symlink Large Caches to Scratch

If you use HuggingFace, Ollama, or other tools that cache large model files, move the cache to scratch and symlink:

```bash
# Move existing cache
mv ~/.cache /scratch/$USER/cache
ln -s /scratch/$USER/cache ~/.cache

# Or set environment variables (add to ~/.bashrc)
export HF_HOME=/scratch/$USER/hf_cache
export TRANSFORMERS_CACHE=/scratch/$USER/hf_cache
```

### Conda Environments

Conda environments (typically at `~/miniconda3/` or `~/anaconda3/`) can grow to tens of GB. Consider:

1. **Exporting environment specs** so they can be recreated: `conda env export > environment.yml`
2. **Cleaning unused packages**: `conda clean --all`
3. **Optionally moving conda to scratch**: Only do this if you're comfortable recreating environments if scratch data is lost.

### Checking Your Usage

```bash
# Home directory breakdown
du -sh ~/*/ 2>/dev/null | sort -rh | head -10

# Scratch usage
du -sh /scratch/$USER

# Hidden directories (often large)
du -sh ~/.[^.]* 2>/dev/null | sort -rh | head -5

# Check your quota status
quota -s
```

---

## Storage Quotas

Quotas exist as a **safety net** to prevent accidental disk-filling incidents (e.g., runaway log files, forgotten data copies). They are not intended to restrict normal use.

### Root Drive (`/`) — Includes `/home` and `/data`

| Role | Hard Cap |
|---|---|
| Faculty | 500GB |
| Postdocs | 500GB |
| PhD students | 500GB |
| Special: power | 800GB |

### Scratch Drive (`/scratch`)

| Scope | Hard Cap |
|---|---|
| Per user | 400GB |

If you hit your hard cap, you will be unable to write new files until you free space. Contact the admin if you believe your cap needs adjusting.

---

## Security

- **Home directories are private.** Other users cannot read your files.
- **Scratch directories are private.** Each user's scratch space is mode 700.
- **Per-job scratch is private.** Created with mode 700, owned by the job submitter.
- **Shared data** in `/data/shared/` is accessible to all members of the `hpc-users` group.
- **Project directories** have per-project access control via dedicated groups.

---

## Quick Reference

| Task | Command |
|---|---|
| Check home usage | `du -sh ~/*/ \| sort -rh` |
| Check scratch usage | `du -sh /scratch/$USER` |
| Check quota | `quota -s` |
| Submit CPU job | `sbatch --partition=winston script.sh` |
| Submit GPU job | `sbatch --partition=winston-gpu --gres=gpu:1 script.sh` |
| Check job queue | `squeue` |
| Cancel a job | `scancel <jobid>` |

---

## Admin Reference

### Adding a New User

```bash
# 1. Create user with private group (Ubuntu adduser does this by default)
sudo adduser <username>

# 2. Add to hpc-users group
sudo usermod -aG hpc-users <username>

# 3. Add to Slurm (adjust account and qos as appropriate)
sudo sacctmgr add user <username> account=phd qos=phd

# 4. Create scratch directory
sudo mkdir -p /scratch/<username>
sudo chown <username>:<username> /scratch/<username>
sudo chmod 700 /scratch/<username>

# 5. Set hard caps
sudo setquota -u <username> 0 500G 0 0 /
sudo setquota -u <username> 0 400G 0 0 /scratch
```

### Creating a Project Directory

```bash
# Private project (single user)
sudo mkdir -p /data/projects/<project_name>
sudo chown <user>:<user> /data/projects/<project_name>
sudo chmod 700 /data/projects/<project_name>

# Collaborative project (multiple users)
sudo groupadd proj-<project_name>
sudo usermod -aG proj-<project_name> <user1>
sudo usermod -aG proj-<project_name> <user2>
sudo mkdir -p /data/projects/<project_name>
sudo chown root:proj-<project_name> /data/projects/<project_name>
sudo chmod 2770 /data/projects/<project_name>
```

### Adjusting a User's Quota

```bash
# Check current quotas
sudo repquota -s /
sudo repquota -s /scratch

# Set new hard cap (example: 600GB on root)
sudo setquota -u <username> 0 600G 0 0 /
```

### Key System Files

| File | Purpose |
|---|---|
| `/etc/fstab` | Drive mount configuration (root + scratch + EFI) |
| `/etc/slurm/slurm.conf` | Slurm configuration (includes Prolog/Epilog) |
| `/etc/slurm/prolog.d/scratch-setup.sh` | Creates per-job scratch directories |
| `/etc/slurm/epilog.d/scratch-cleanup.sh` | Cleans up per-job scratch directories |
| `/usr/local/bin/home-usage-check.sh` | Daily home directory usage checker |
| `/usr/local/bin/scratch-stale-check.sh` | Weekly stale scratch file checker |
| `/etc/profile.d/storage-notice.sh` | Login hook that displays storage warnings |
| `/etc/cron.d/home-usage-check` | Cron schedule for home usage checker (daily 4am) |
| `/etc/cron.d/scratch-stale-check` | Cron schedule for stale file checker (Sunday 3am) |

### Symlinks in Place

| Symlink | Target | Reason |
|---|---|---|
| `/data/spack` | `/scratch/spack` | Moved to free root drive space |
| `/usr/share/ollama` | `/scratch/ollama` | Moved to free root drive space |
| `/home/power/endow` | `/data/projects/endow` | Logical separation of large dataset |
| `/home/robins75/scratch` | `/scratch/robins75` | Working data on scratch drive |

### Known Issues (as of 2026-03-17)

- **NVIDIA DKMS build failure on kernel 6.17.0-14-generic.** The `nvidia-570-open` driver cannot compile against this kernel's headers. Do NOT reboot into this kernel. The currently running kernel is unaffected. Resolution: pin the current kernel, remove the problematic one, or wait for an updated NVIDIA driver package.
- **sturgis** has 238GB in `~/.cache` (likely HuggingFace models). Needs to move to `/scratch/sturgis/cache` and symlink.
- **decourson** is at 155GB on root. On a temporary allowance — follow up to reduce.
