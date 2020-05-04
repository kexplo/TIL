# MicroK8s

Lightweight Kubernetes cluster with a single-node.

https://microk8s.io/

> If you need a more lighter Kubernetes cluster (even including IoT), see https://k3s.io/

## Installation

```bash
# Install microk8s from Snap
$ sudo snap install microk8s --classic
# Use microk8s command without sudo (re-login required)
$ sudo usermod -a -G microk8s $USER
# Add alias for microk8s.kubectl
$ alias kubectl='microk8s.kubectl'
```

## Quickstarts

- `microk8s.status`: Show status
- `microk8s.enable <addon>`: Enable microk8s addon
- `microk8s.disable <addon>`: Disable microk8s addon
- `microk8s.kubectl`: Run kubectl

## Recommend addons

- dns (CoreDNS)
- helm
- metallb (MetalLB)

link: [list of all Microk8s addons](https://microk8s.io/docs/addons)

## Troubleshooting

- `microk8s.inspect`: Show System status and collecting logs

SEE: https://microk8s.io/docs/troubleshooting

----

## Do not expose NodePorts externally

`serviceType=NodePort` or `serviceType=LoadBalancer` will open the NodePort externally, even if the firewall(e.g., ufw) was enabled.

[Because `kube-proxy` is writing `iptables` rules.](https://stackoverflow.com/a/53142983)

It can't close but it can set does not expose externally.

```
# append below line to /var/snap/microk8s/1378/args/kube-proxy
--nodeport-addresses=["::1/128","127.0.0.1/32"]
```

reference: https://github.com/kubernetes/kubernetes/pull/89998#issuecomment-611590526