# Traefik

## Install Traefik on Microk8s (using helm)

```bash
# Set externalIP manually
$ helm install stable/traefik --name traefik --set externalIP=<external-ip> --namespace kube-system
```

or

```bash
# Deploy as NodePort
$ helm install stable/traefik --name traefik --set serviceType=NodePort --namespace kube-system

# Show NodeIP and NodePort
$ kubectl describe svc traefik --namespace kube-system
```

You can also use `serviceType=LoadBalancer` with the LoadBalancer.

### Enable Dashboard and set Dashboard domain

1. Append below options to `helm install stable/traefik ...` command

   ```
   --set dashboard.enabled=true,dashboard.domain=traefik-dashboard.local
   ```

2. Confiure DNS record. (/etc/hosts)

   ```bash
   $ cat '<external-ip or NodeIP> traefik-dashboard.local' >> /etc/hosts
   ```

## Routing

See: https://docs.traefik.io/v1.7/user-guide/kubernetes/

### Name-based Routing

```yml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: cheese
  annotations:
    kubernetes.io/ingress.class: traefik
spec:
  rules:
  - host: stilton.minikube
    http:
      paths:
      - backend:
          serviceName: stilton
          servicePort: http
  - host: cheddar.minikube
    http:
      paths:
      - backend:
          serviceName: cheddar
          servicePort: http
  - host: wensleydale.minikube
    http:
      paths:
      - path: /
        backend:
          serviceName: wensleydale
          servicePort: http
```

### Path-based Routing

```yml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: cheeses
  annotations:
    kubernetes.io/ingress.class: traefik
    traefik.frontend.rule.type: PathPrefixStrip
spec:
  rules:
  - host: cheeses.minikube
    http:
      paths:
      - path: /stilton
        backend:
          serviceName: stilton
          servicePort: http
      - path: /cheddar
        backend:
          serviceName: cheddar
          servicePort: http
      - path: /wensleydale
        backend:
          serviceName: wensleydale
          servicePort: http
```
