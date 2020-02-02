# SSH Proxy settings

Create a user for SSH proxy

```bash
$ sudo useradd -s /bin/false tunnel
```

Edit `/etc/ssh/sshd_config`

```sshd_config
Match User tunnel
    X11Forwarding no
    PermitTTY no
    PermitTunnel no
    AllowAgentForwarding no
    GatewayPorts no
    AllowTcpForwarding yes
    AuthorizedKeysFile|·/etc/ssh/authorized_keys_%u
```

```bash
$ ssh-keygen -t rsa -b 4096 -C "tunnel"
$ sudo cat id_rsa >> /etc/ssh/authorized_keys_tunnel
$ sudo systemctl restart ssh
```

----

Connect to the server

```bash
$ ssh -N -D <port> -C tunnel@host
```

- `-D <port>`: Open a SOCKS proxy on local port `<port>`.
- `-C`: Requests compression of all data.
- `-q`: Quiet mode.
- `-N`: Do not execute a remote command.  This is useful for just forwarding ports.
