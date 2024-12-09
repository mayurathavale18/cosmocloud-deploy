# Cosmocloud Helm Chart

This Helm chart deploys a sample application stack with a frontend, backend, and Redis database using Kubernetes.

## Components
- **Frontend:** Exposed via NodePort (5173 → 31000)
- **Backend:** Exposed via ClusterIP (Port 8000)
- **Redis:** Exposed via ClusterIP (Port 6379)

## Environment Variables
- **Frontend:** `BACKEND_URL=http://backend-svc:8000`
- **Backend:** `REDIS_URI=redis://redis-svc:6379`

## Deployment Instructions

1. Clone the repository:
   ```bash
   git clone https://github.com/mayurathavale18/cosmocloud-deploy.git
   ```

2. Deploy the Helm chart:
   ```bash
   helm install testapp cosmocloud-deploy --atomic --timeout 30s
   ```

3. Verify the deployment:
   ```bash
   kubectl get pods
   kubectl get svc
   ```

4. Access the frontend:
   - If using **Minikube**:
     ```bash
     minikube service frontend-svc --url
     ```
   - Or, use port forwarding:
     ```bash
     kubectl port-forward svc/frontend-svc 31000:5173
     ```

## Clean Up
To delete the deployment:
```bash
helm uninstall testapp
```

### Note on Timeout Setting
During testing, the application stack required more than 30 seconds to initialize due to resource startup times. The `helm install` command works successfully with a 2-minute timeout:
```bash
helm install testapp cosmocloud-deploy --atomic --timeout 2m
```


---

### Optimize Resource Configuration (Optional)
If time permits, try to:
1. **Reduce container startup time** by inspecting logs and optimizing application configurations.
2. **Pre-pull images** on nodes to reduce image download times:
   ```bash
   kubectl create -f pre-pull.yaml
   ```
### Example:
apiVersion: v1
kind: Pod
metadata:
  name: image-puller
spec:
  containers:
  - name: puller
    image: shreybatra/sample-backend
    command: [ "sleep", "3600" ]


