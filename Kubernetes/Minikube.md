# Minikube

## Run minkube without VM

No. Please do not do this, ever.

minikube was designed to run Kubernetes within a dedicated VM, and when used with --vm-driver=none, may overwrite system binaries, configuration files, and system logs. Executing minikube --vm-driver=none outside of a VM could result in data loss, system instability and decreased security.

### references

- https://github.com/kubernetes/minikube/issues/2575
- https://github.com/kubernetes/minikube/blob/master/docs/vmdriver-none.md