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
