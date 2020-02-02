# WireGuard

https://www.wireguard.com/

## Install WireGuard on Ubuntu

Install WireGuard.

```bash
$ sudo add-apt-repository ppa:wireguard/wireguard
$ sudo apt install wireguard
```

Enable IP Forwarding. Open `/etc/sysctl.conf` and uncomment the `#net.ipv4.ip_forward=1` line.

```conf
net.ipv4.ip_forward=1
```

## Create Private and Public keys

```bash
$ umask 077
$ wg genkey | tee privatekey | wg pubkey > publickey
```

## Configure Server

```conf
# /etc/wireguard/wg0.conf

[Interface]
Address = <Private Address for VPN. e.g., 10.10.0.1/24>
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE; ip6tables -A FORWARD -i wg0 -j ACCEPT; ip6tables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE; ip6tables -D FORWARD -i wg0 -j ACCEPT; ip6tables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
ListenPort = 51820
PrivateKey = <Server Private Key>

[Peer]
PublicKey = <Client Public Key>
AllowedIPs = <Client Unique IP. e.g., 10.10.0.2/32>
```

> Other iptables rule:
>
> Copied from https://www.ckn.io/blog/2017/11/14/wireguard-vpn-typical-setup/
>
> ```
> # Track VPN connection
> iptables -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
> iptables -A FORWARD -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
> 
> # Allowing incoming VPN traffic on the listening port
> iptables -A INPUT -p udp -m udp --dport 51820 -m conntrack --ctstate NEW -j ACCEPT
> 
> # Allow both TCP and UDP recursive DNS traffic
> iptables -A INPUT -s 10.200.200.0/24 -p tcp -m tcp --dport 53 -m conntrack --ctstate NEW -j ACCEPT
> iptables -A INPUT -s 10.200.200.0/24 -p udp -m udp --dport 53 -m conntrack --ctstate NEW -j ACCEPT
>
> # Allow forwarding of packets that stay in the VPN tunnel
> iptables -A FORWARD -i wg0 -o wg0 -m conntrack --ctstate NEW -j ACCEPT
>
> # Set up nat
> iptables -t nat -A POSTROUTING -s 10.200.200.0/24 -o eth0 -j MASQUERADE

If you want to add a new client, add a new `[Peer]` section to `wg0.conf`

## Enable/Diasble the WireGuard interface on the Server

```bash
# Enable WireGuard interface
$ wg-quick up wg0

# Disable WireGuard interface
$ wg-quick down wg0
```

```bash
# Enable the interface as a service.
$ systemctl enable wg-quick@wg0.service
```

## Show status

```bash
$ sudo wg show
interface: wg0
  public key: <Server Public Key>
  private key: (hidden)
  listening port: <Listen Port>
```

----

## Configure Client

- [WireGuard for Windows](https://www.wireguard.com/install/) from the homepage.
- [WireGuard for Android](https://play.google.com/store/apps/details?id=com.wireguard.android) from PlayStore.

Client config:

```conf
[Interface]
PrivateKey = <Client Private Key>
Address = <Client IP. e.g., 10.10.0.2/32>

[Peer]
PublicKey = <Server Public Key>
AllowedIPs = 0.0.0.0/0
Endpoint = <Server IP:Port>
```

## References

- https://www.ckn.io/blog/2017/11/14/wireguard-vpn-typical-setup/
- https://golb.hplar.ch/2018/10/wireguard-on-amazon-lightsail.html