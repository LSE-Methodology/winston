# SSH tips

## Make a shortcut host entry

If you don’t want to type the IP every time, add a host alias to your ssh config:

```bash
nano ~/.ssh/config
```

Then add:

```bash
Host winston
    HostName <ip_address>
    User <username>
```

Then connect with:

```bash
ssh winston
```
