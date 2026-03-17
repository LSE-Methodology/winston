# Transferring files

You can move data to/from Winston using either GUI tools or command-line tools.

## GUI option

A tool like Cyberduck (SFTP) can transfer files to your user space on Winston using:

- your username
- your authentication method (password or key)
- the Winston host (provided privately)

## Command line: `scp`

Copy local → Winston:

```bash
scp -r <path/to/local/files> <username>@<host-provided-by-admin>:<remote/path>
```

Copy Winston → local (reverse direction):

```bash
scp -r <username>@<host-provided-by-admin>:<remote/path> <path/to/local/folder>
```

Notes:
- `-r` recursively copies directories.
- Use absolute paths to avoid surprises (e.g. `/home/<username>/project` or `/scratch/<username>/data`).

## Command line: `rsync` (recommended for lots of files)

rsync is faster for large transfers and incremental updates (only changed files are sent).

```bash
rsync -av <path/to/local/files> <username>@<host-provided-by-admin>:<remote/path>
```

Reverse direction (Winston → local):

```bash
rsync -av <username>@<host-provided-by-admin>:<remote/path> <path/to/local/folder>
```

Common flags:
- `-a` preserve metadata (permissions, timestamps)
- `-v` verbose output
- add `-z` to compress during transfer (useful over slower links)
- trailing slash matters: `data/` copies contents; `data` copies the folder
