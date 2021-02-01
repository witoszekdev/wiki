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

### Sources

[https://phoenixnap.com/kb/ssh-port-forwarding](https://phoenixnap.com/kb/ssh-port-forwarding)