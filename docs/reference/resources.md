# Cluster resources

## Compute

- ~192 vCPUs (188 allocatable)
- ~250GB RAM
- 2 × RTX 6000 Ada Generation (48GB VRAM each)

### Memory defaults

By default, Slurm jobs are assigned **1GB RAM per CPU core** unless you request more.

### Checking availability

```bash
winfo
```

!!! note
    `winfo` is a local helper; you can also use `sinfo` and `squeue` for Slurm-wide views.

## Storage

Winston has two NVMe drives:

| Drive | Capacity | Mount | Purpose |
|---|---|---|---|
| Micron 2048GB | 1.9TB | `/` (root) | OS, home directories, `/data` |
| Samsung 1024GB | 954GB | `/scratch` | Working data, active job data |

### Directory structure

```
/home/<username>/          Code, configs, environments (persistent)
/scratch/<username>/       Large files, active working data (persistent)
/scratch/<username>/job_N/ Auto-created per Slurm job, auto-deleted on completion
/data/shared/              Shared datasets (read/write for all hpc-users)
/data/projects/<name>/     Private or collaborative project directories
```

### Where to put what

| Location | Use for | Avoid |
|---|---|---|
| `/home/<username>/` | Source code, scripts, conda environments, configs, small files | Large datasets, model checkpoints, HuggingFace/Ollama caches |
| `/scratch/<username>/` | Large datasets, model outputs, cache directories (symlinked from `~/.cache`) | — |
| `/scratch/<username>/job_N/` | Throwaway intermediate files during a job | Anything you need after the job ends |
| `/data/shared/` | Datasets multiple users need | Personal files |
| `/data/projects/<name>/` | Persistent project data shared across collaborators | — |

### Storage quotas

Quotas are a **safety net** to prevent accidental disk-filling incidents, not a usage target.

**Root drive (`/`)** — includes `/home` and `/data`:

| Role | Hard cap |
|---|---|
| Faculty | 500GB |
| Postdocs | 500GB |
| PhD students | 500GB |

**Scratch drive (`/scratch`)**:

| Scope | Hard cap |
|---|---|
| Per user | 400GB |

If you hit your hard cap you cannot write new files until you free space. Contact the admin if your cap needs adjusting.

### Home directory warnings

Usage warnings are displayed at login when your home directory exceeds **50GB**, **100GB**, or **200GB**.

### Scratch stale file notices

Files on `/scratch` untouched for **6+ months** will trigger a login reminder asking you to archive or remove them. Nothing is automatically deleted.

### Security

- Home directories are **private** (other users cannot read your files).
- Scratch directories are **private** (mode 700).
- Per-job scratch is **private** (mode 700, owned by the job submitter).
- `/data/shared/` is accessible to all `hpc-users` group members.
- `/data/projects/` directories have per-project access control.
