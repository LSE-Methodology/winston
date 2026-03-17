# SSH tips

## Make a shortcut host entry

If you don’t want to type the IP every time, add a host alias to your ssh config:

```bash
nano ~/.ssh/config
```

Then add:

```bash
Host winston
    HostName <host-provided-by-admin>
    User <username>
    IdentityFile ~/.ssh/id_ed25519
    ServerAliveInterval 60
    ServerAliveCountMax 2
```

Then connect with:

```bash
ssh winston
```

!!! note
    Remove `IdentityFile` if you are using password-based login.
