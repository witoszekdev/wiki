## Port forwarding

### Local

server -> you machine
Exposes ports from server to your local machine (or some other)

```
ssh -L LOCAL_PORT:localhost:REMOTE_PORT root@example.com
```

### Remote

your machine -> server
Exposes port from your machine to the server (great for expsing from NAT)

```
ssh -R REMOTE_PORT:localhost:REMOTE_PORT root@example.com
```

Run command on the server:

```
ssh -tR REMOTE_PORT:localhost:LOCAL_PORT root@example.com python3 ~/sirtunnel.py domain.example.com REMOTE_PORT
```
Taken from [SirTunnel](https://github.com/anderspitman/SirTunnel)

### Sources

[https://phoenixnap.com/kb/ssh-port-forwarding](https://phoenixnap.com/kb/ssh-port-forwarding)
