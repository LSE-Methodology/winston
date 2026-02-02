# Software management overview

Winston is configured for **user-managed software installations**.
You do not need administrator privileges (`sudo`) to install your own tools.

Two supported approaches:

- **Spack**: compiled system software (R, compilers, MPI, scientific libraries)
- **Miniconda**: user-level environments (Python/R packages, data science stacks)

## System overview

| Layer | Tool | Purpose |
|---|---|---|
| Shared base layer | Spack (`/opt/spack`) | Core system software (R, compilers, MPI, libraries) |
| User build area | `/data/spack/users/$USER` | Your Spack-built software install tree |
| User environments | Miniconda (`~/miniconda3`) | Python/R environments + packages |
| Scheduler | Slurm | Runs jobs on compute nodes |
