# Pods

Pods are the **smallest deployable unit** in Kubernetes. Every application running in a Kubernetes cluster runs inside a Pod.

---

## What is a Pod?

A Pod is a wrapper around one or more containers that:

* Share the same network namespace (same IP address).
* Share storage volumes.
* Are always scheduled on the same node.
* Have the same lifecycle.

> **Note**
>
> Although a Pod can contain multiple containers, the most common practice is to run **one container per Pod**.

---

## Pod Architecture

```text
+--------------------------------------+
|                Pod                   |
|                                      |
|  +-------------------------------+   |
|  |        Container (nginx)      |   |
|  +-------------------------------+   |
|                                      |
|  Shared Network & Shared Storage     |
+--------------------------------------+
```

---

## Create a Pod

```bash
kubectl run my-pod --image=nginx --port=80
```

This command creates a Pod named **my-pod** using the **nginx** image.

---

## Generate Pod YAML (Without Creating the Pod)

Sometimes you may want to generate the YAML manifest before creating the resource.

```bash
kubectl run nginx \
--image=nginx \
--port=80 \
--dry-run=client \
-o yaml > pod.yaml
```

This command:

* Generates the Pod manifest.
* Does **not** create the Pod.
* Saves the YAML to `pod.yaml`.

---

## Example Pod YAML

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx-pod

spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

---

## Common Pod Commands

### List all Pods

```bash
kubectl get pods
```

---

### List Pods with more information

```bash
kubectl get pods -o wide
```

Displays additional information such as:

* Pod IP
* Node Name
* Internal IP
* Container Image

---

### Describe a Pod

```bash
kubectl describe pod <pod-name>
```

Displays detailed information including:

* Labels
* Events
* Container status
* Mounted volumes
* Node information
* Resource requests and limits

---

### View Pod Logs

```bash
kubectl logs <pod-name>
```

Example:

```bash
kubectl logs nginx-pod
```

---

### Execute a Command Inside a Pod

```bash
kubectl exec -it <pod-name> -- /bin/bash
```

If Bash is unavailable:

```bash
kubectl exec -it <pod-name> -- /bin/sh
```

---

### Delete a Pod

```bash
kubectl delete pod <pod-name>
```

Example:

```bash
kubectl delete pod nginx-pod
```

---

## Pod Lifecycle

A Pod goes through different phases during its lifetime.

```
Pending
    │
    ▼
Running
    │
    ▼
Succeeded / Failed
```

### Pod Phases

| Phase       | Description                                               |
| ----------- | --------------------------------------------------------- |
| `Pending`   | Pod has been accepted but containers are not yet running. |
| `Running`   | All containers are running successfully.                  |
| `Succeeded` | All containers completed successfully and exited.         |
| `Failed`    | One or more containers terminated with an error.          |
| `Unknown`   | Kubernetes cannot determine the Pod's current state.      |

---

## Important Points

* A Pod is the smallest deployable unit in Kubernetes.
* Every Pod has its own unique IP address.
* Pods are **ephemeral** (temporary) and can be recreated at any time.
* Pods should generally **not** be created directly for production workloads.
* In production, Pods are typically managed by higher-level controllers such as:

  * Deployment
  * ReplicaSet
  * StatefulSet
  * DaemonSet
  * Job
  * CronJob

---

## Best Practices

* Create Pods through a **Deployment** rather than directly.
* Use labels to organize and select Pods.
* Store configuration in **ConfigMaps** and sensitive information in **Secrets**.
* Set CPU and memory requests/limits for production workloads.
* Keep one main application container per Pod whenever possible.

---

## Quick Reference

| Command                                    | Description                       |
| ------------------------------------------ | --------------------------------- |
| `kubectl run my-pod --image=nginx`         | Create a Pod                      |
| `kubectl get pods`                         | List all Pods                     |
| `kubectl get pods -o wide`                 | List Pods with additional details |
| `kubectl describe pod <pod-name>`          | Show detailed Pod information     |
| `kubectl logs <pod-name>`                  | Display Pod logs                  |
| `kubectl exec -it <pod-name> -- /bin/bash` | Open a shell inside a Pod         |
| `kubectl delete pod <pod-name>`            | Delete a Pod                      |
