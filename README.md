### Kubernetes Deployment: NGINX Hello App

This document describes the steps taken to deploy the `nginxdemos/hello` application on a Kubernetes cluster using Deployment, NodePort, ConfigMap, health checks, and TLS with Ingress.

---

#### 1. Create Deployment

Apply the deployment YAML:
```bash
kubectl apply -f deployment.yaml
```

---

#### 2. Create ConfigMap for Custom NGINX Config

For getting the nginx config from the docker image we can use the below command:

```bash
docker run --rm nginxdemos/hello cat /etc/nginx/conf.d/hello.conf > nginx-hello.conf
```

Apply the ConfigMap:
```bash
kubectl apply -f configmap.yaml
```
---

#### 3. Expose the App with NodePort

Apply the service:
```bash
kubectl apply -f service.yaml
```
Get the minikube IP using:

```bash
minikube ip
```

Access the app:
```
http://<minikubeIP>:<NodePort>
```

---

#### 4. TLS Setup with Ingress

Generate self-signed certificate:
```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout kloudspot-tls.key -out kloudspot-tls.crt -subj "/CN=kloudspot.local"
```
To get the encoded key and certficate:

```bash
base64 -w 0 kloudspot-tls.key > encoded-kloudspot-tls.key

base64 -w 0 kloudspot-tls.crt > encoded-kloudspot-tls.crt
```

Create TLS secret:
```bash
kubectl apply -f tls-secret.yaml
```

Apply the Ingress resource:
```bash
kubectl apply -f ingress.yaml
```

Update `/etc/hosts`:
```
<MinikubeIP> kloudspot.local
```

Access via HTTPS:
```
https://kloudspot.local
```

---

#### 5. Debugging Tips

Check pod logs:
```bash
kubectl logs <pod-name>
```

To describe the pods:
```bash
kubectl describe pod <pod-name> -n kloudspot
```

---

#### Summary

- Deployed with 2 replicas
- Exposed externally using NodePort
- Custom NGINX config via ConfigMap
- Health probes for liveness and readiness
- Optional TLS setup with Ingress

#### URL for the task video: 
```bash
https://drive.google.com/file/d/1ZXE9GVQUd-5-vitHX1NxOYIXpXRdRm9f/view
```

This provides a full Kubernetes setup to deploy and access an NGINX-based demo app.
