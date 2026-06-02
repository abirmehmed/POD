# DevOps & Production Infrastructure: Set 5
## Kubernetes Fundamentals for Laravel & Container Orchestration

## 📘 10 Conceptual Questions
1. **Pods vs. Deployments:** What is a Pod, and why do we almost never create raw Pods in production? Explain how Deployments and ReplicaSets provide self-healing, scaling, and rolling updates.
2. **Stateless vs. Stateful in K8s:** Why is a typical Laravel app considered "stateless" and perfectly suited for Kubernetes? How do you handle stateful requirements like file uploads (`storage/app/public`) and session data in a multi-pod environment?
3. **Services & Networking:** How do PHP pods discover and talk to MySQL/Redis pods? Explain the difference between `ClusterIP` (internal), `NodePort`, and `LoadBalancer` (external) Services.
4. **ConfigMaps vs. Secrets:** How do you manage a Laravel `.env` file in Kubernetes? What is the technical difference between a ConfigMap and a Secret, and why are Secrets *not* fully encrypted by default without enabling encryption at rest?
5. **Liveness vs. Readiness Probes:** What is the exact difference between these two health checks? How does a misconfigured Readiness probe cause zero-downtime deployments to fail, and how does a bad Liveness probe cause a "CrashLoopBackOff"?
6. **Ingress Controllers:** How does Kubernetes route external HTTP traffic to different internal services? Why do you need an Ingress Controller (like Nginx or Traefik) to handle host-based routing (`multimart.com` vs `api.multimart.com`) and TLS termination?
7. **Resource Requests vs. Limits:** What is the difference between `requests` and `limits` for CPU and Memory? How does setting these incorrectly lead to `OOMKilled` (Out of Memory) pods or CPU throttling in PHP-FPM?
8. **Init Containers:** How do you run `php artisan migrate` safely in Kubernetes before the main application starts serving traffic? Why shouldn't the main app container run migrations on startup?
9. **Horizontal Pod Autoscaler (HPA):** How does Kubernetes decide to add more PHP pods? What metrics (CPU, Memory, or custom queue depth) trigger scaling, and how does PHP-FPM's `pm.max_children` setting interact with K8s resource limits?
10. **Helm Charts:** What is Helm, and why is it called the "package manager for Kubernetes"? How do "Charts" and `values.yaml` files solve the problem of managing dozens of raw YAML files across Dev, Staging, and Prod?

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Setup Local Kubernetes Cluster**<br>Install Minikube, Kind, or K3s on your local machine. Install `kubectl`. Start the cluster and verify you can run `kubectl get nodes`. Deploy a basic Nginx pod and access it via port-forwarding. | Cluster is running. `kubectl` is configured. Nginx pod is accessible locally. |
| 2 | **Deploy Laravel via Deployment**<br>Write a `deployment.yaml` for your MultiMart app. Set `replicas: 2`. Use the Docker image you built in Set 2. Expose port 80/9000. Apply it and verify two pods are running. | Two Laravel pods are running. `kubectl get pods` shows `2/2 Ready`. They are using the correct image. |
| 3 | **Service & Ingress Exposure**<br>Create a `Service` (ClusterIP) for your Laravel deployment. Create an `Ingress` resource to route traffic from `multimart.local` to the service. Configure your local `/etc/hosts` to test it in the browser. | Browser navigates to `multimart.local` and loads MultiMart. Ingress routes traffic correctly to the backend pods. |
| 4 | **ConfigMap & Secret Management**<br>Convert your Laravel `.env` into a K8s `Secret` (for DB passwords, APP_KEY) and `ConfigMap` (for APP_ENV, APP_NAME). Mount them as environment variables in your Deployment YAML. | No hardcoded credentials in YAML. `kubectl exec` into the pod shows environment variables are injected correctly. |
| 5 | **Persistent Volume for Storage**<br>Create a `PersistentVolumeClaim` (PVC) for Laravel's `storage` directory. Mount it to the PHP pod. Upload an image via MultiMart, delete the pod, and verify the image persists when the new pod starts. | File uploads survive pod restarts. PVC is bound to a PV. Multi-pod file sharing works (or uses ReadWriteMany). |
| 6 | **Liveness & Readiness Probes**<br>Configure HTTP GET probes in your Deployment. Readiness checks `/` (or a health route). Liveness checks a deeper route. Simulate a failure and watch Kubernetes restart the pod. | K8s stops sending traffic to unready pods. Failing liveness triggers an automatic pod restart. |
| 7 | **Init Container for Migrations**<br>Add an `initContainers` block to your Deployment that runs `php artisan migrate --force`. Verify the main PHP-FPM container does not start until migrations complete successfully. | `kubectl get pods` shows `Init:0/1` until migrations finish. If DB is down, init container fails and blocks app startup. |
| 8 | **Horizontal Pod Autoscaling (HPA)**<br>Create an HPA resource to scale your Laravel deployment between 2 and 5 replicas based on CPU usage. Use a load testing tool (like `hey` or `ab`) to spike traffic and watch the pods scale up. | `kubectl get hpa` shows current metrics. Pod count increases automatically under load and scales down when traffic stops. |
| 9 | **Resource Limits & OOM Testing**<br>Set strict memory limits (e.g., `256Mi`) on your pod. Write a quick script to make PHP consume 300MB of memory. Watch the pod get `OOMKilled` in `kubectl describe pod`. Fix by adjusting limits. | Pod is killed and restarted automatically when limit is hit. You understand how to right-size PHP-FPM memory limits. |
| 10| **Basic Helm Chart Creation**<br>Wrap your Deployment, Service, and Ingress YAMLs into a basic Helm chart (`helm create multimart`). Use `values.yaml` to manage image tags and replica counts. Deploy using `helm install`. | `helm install multimart ./chart` deploys the app. Changing `values.yaml` and running `helm upgrade` updates the app without touching raw YAMLs. |
