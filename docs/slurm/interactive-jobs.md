# Interactive jobs

Interactive jobs let you reserve resources and work “live” on a compute node.

## Request an interactive allocation

```bash
salloc --mem=10G --cpus-per-task=4 --time=01:00:00
```

Once allocated, run your commands as normal (on the allocated node).

!!! tip
    If you need interactive work frequently, consider using tmux so sessions survive disconnects.

## One-shot interactive shell

You can also request a shell in one command:

```bash
srun --pty --mem=10G --cpus-per-task=4 --time=01:00:00 bash
```
