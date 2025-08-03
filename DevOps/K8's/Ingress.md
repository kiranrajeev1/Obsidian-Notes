# Kubernetes – Ingress

🗓️ **Date:** 04-08-2025  
🕒 **Time:** 00:00  

🏷️ **Tags:** #kubernetes #devops #Ingress  

---

## 📝 Notes

##### Kubernetes Ingress

###### Overview
Ingress is an API object that manages **external access** to services in a Kubernetes cluster, typically **HTTP(S)** traffic. It provides routing rules to expose services using hostnames or paths without requiring external LoadBalancers per service.

---

##### Key Concepts

###### Ingress Resource
Defines routing rules for traffic coming into the cluster.

###### Ingress Controller
A Kubernetes controller that **fulfills the Ingress**. It watches Ingress resources and configures a reverse proxy accordingly (e.g., NGINX, Traefik, HAProxy).

---

##### Ingress Workflow

1. Client sends request to external IP of Ingress Controller    
2. Controller receives the request and matches it against Ingress rules
3. Request is forwarded to the backend `Service`
4. Service routes traffic to backend `Pod`

---

##### Basic Ingress YAML Example

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  namespace: default
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /app
        pathType: Prefix
        backend:
          service:
            name: app-service
            port:
              number: 80
```

---

##### Ingress Path Types

| Path Type                | Description                                    |
| ------------------------ | ---------------------------------------------- |
| `Prefix`                 | Matches path prefix (e.g. `/app`, `/app/test`) |
| `Exact`                  | Matches exactly the path string                |
| `ImplementationSpecific` | Behavior depends on Ingress Controller         |

---

##### Annotations (Common for NGINX)

| Annotation                                     | Purpose                        |
| ---------------------------------------------- | ------------------------------ |
| `nginx.ingress.kubernetes.io/rewrite-target`   | Rewrites incoming paths        |
| `nginx.ingress.kubernetes.io/ssl-redirect`     | Forces HTTPS                   |
| `nginx.ingress.kubernetes.io/backend-protocol` | Specify HTTP/HTTPS for backend |
| `nginx.ingress.kubernetes.io/auth-type`        | Enables basic auth             |

---

##### TLS in Ingress

```yaml
spec:
  tls:
  - hosts:
    - example.com
    secretName: example-tls
```

TLS secrets must be in the same namespace as the Ingress resource.

---

##### Ingress Controller Deployment

Example (NGINX Ingress):

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.1/deploy/static/provider/cloud/deploy.yaml
```

This deploys the Ingress controller to the cluster. Only **Ingress Controllers** can implement the `Ingress` API.

---

##### Common Use Cases

- Route traffic by domain (`foo.example.com`, `bar.example.com`)
- Route traffic by path (`/api`, `/web`)
- Use HTTPS with TLS
- Add authentication and rate limiting via annotations

---

##### Useful Commands

```bash
kubectl get ingress -n default
kubectl describe ingress example-ingress
kubectl get svc -n ingress-nginx
kubectl get pods -n ingress-nginx
```

---

##### Debugging Tips

- Check if the Ingress Controller is running and healthy
- Use `kubectl describe ingress` to inspect routing rules
- Confirm DNS points to the Ingress Controller's external IP
- Check annotations are supported by your specific Ingress Controller
- Use `curl -v` or browser dev tools to trace request behavior    

---

##### Alternatives to Ingress

|Tool|Description|
|---|---|
|`Service of type LoadBalancer`|Direct exposure via cloud load balancer|
|`NodePort`|Expose via static port on all nodes|
|`Gateway API`|Next-gen standard replacing Ingress (more extensible)|

---

##### Summary

- Ingress enables HTTP/HTTPS routing to internal services via a single entry point
- Requires an Ingress Controller to function (e.g., NGINX, Traefik)
- Supports domain-based and path-based routing
- Can use TLS and annotations for advanced behavior
- Must be deployed with attention to networking setup and controller-specific config

```mermaid
flowchart TD
    A[Client (Browser / HTTP)] --> B[Ingress Controller (e.g., NGINX)]
    B --> C{Ingress Rules Match?}
    C -->|Match Found| D[Forward to Service]
    D --> E[Service Routes to Pod]
    C -->|No Match| F[Return 404 Not Found]
    B --> G[Handles TLS/SSL if Configured]
```


---

## 🧾 Commands

```bash
# Example:
kubectl get pods
```
