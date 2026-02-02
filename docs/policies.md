# Policies and good practice

## Fair usage

- Do not run intensive workloads on the login node (small tests are fine).
- Use Slurm for anything requiring >2 cores or >4GB RAM.

## Software installs

- Keep Spack builds under `/data/spack/users/$USER`.
- Do not modify anything under `/opt/spack`.
- Remove unused Conda environments occasionally.
- Export environments for reproducibility:
  - `conda env export > env.yml`
  - `spack find --format '{name}@{version}' > spack-env.txt`

## Time limits

- Always set `#SBATCH --time` realistically.
- Jobs exceeding walltime will be terminated.
