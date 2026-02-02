# Spack

Spack manages compiled system-level software such as R, compilers, MPI, and scientific libraries.

## Shell setup

Add to your `~/.bashrc` (if not already present):

```bash
# Load Spack
if [ -f /opt/spack/share/spack/setup-env.sh ]; then
    source /opt/spack/share/spack/setup-env.sh
fi
```

Reload your shell:

```bash
source ~/.bashrc
```

## Installing and loading packages

Example:

```bash
spack install r@4.4.1
spack load r@4.4.1
R --version
```

Notes:
    - Builds go under: /data/spack/users/$USER/opt/spack
    - Remove availability in your shell with: `spack unload <pkg>`
    - List installed packages: `spack find`

!!! note
    If a package already exists on the system, Spack may use a prebuilt binary automatically.
