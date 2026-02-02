# Transferring files

You can move data to/from Winston using either GUI tools or command-line tools.

## GUI option

A tool like CyberDuck can transfer files to your user space on Winston using:

- your username
- your authentication method (password or key)
- the Winston host/IP

## Command line: `scp`

Copy local → Winston:

```bash
scp -r <path/to/local/files> <username>@158.143.30.9:<remote/path>
```

Copy Winston → local (reverse direction):

```bash
scp -r <username>@winston.ip.address:<remote/path> <path/to/local/folder>
```

Notes:
    - `-r` recursively copies directories

## Command line: rsync (recommended for lots of files)

rsync is faster for large transfers and incremental updates (only changed files are sent).

```bash
rsync -av <path/to/local/files> <username>@<ip_address>:<remote/path>
```

Common flags:
    - `-a` preserve metadata (permissions, timestamps)
    - `-v` verbose output
    - add `-z` to compress during transfer (useful over slower links)
