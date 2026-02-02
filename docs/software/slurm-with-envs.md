# Using Spack/Conda inside Slurm jobs

You can use Spack and Conda together in the same job.

## Example batch script

`job.sbatch`:

```bash
#!/bin/bash
#SBATCH --job-name=test
#SBATCH --time=00:05:00
#SBATCH --ntasks=1
#SBATCH --output=output_%j.log

# Set up Spack + Conda
source /opt/spack/share/spack/setup-env.sh
source ~/miniconda3/etc/profile.d/conda.sh

conda activate py311
spack load r

Rscript myscript.R
```

Submit:

```bash
sbatch job.sbatch
```

!!! tip
Spack is great for optimized compiled libraries; Conda is great for high-level packages.
