# Deployments

A **Deployment** is a Kubernetes controller that manages the lifecycle of applications by creating and managing **ReplicaSets**, which in turn manage Pods.

A Deployment is the **recommended way** to deploy stateless applications in Kubernetes because it provides features such as scaling, rolling updates, and rollbacks.

> **Note**
>
> In production environments, applications are almost always deployed using **Deployments** instead of creating Pods or ReplicaSets directly.

---

## Deployment Architecture

A Deployment does not create Pods directly.

```text
Deployment
      │
      ▼
ReplicaSet
      │
      ▼
Pods
```

Whenever you create a Deployment:

1. Kubernetes creates a ReplicaSet.
2. The ReplicaSet creates the required number of Pods.
3. The ReplicaSet continuously monitors the Pods and recreates them if they fail.

---

## Why Do We Need Deployments?

Imagine you have an application with three replicas.

```text
                Deployment
                     │
                     ▼
               ReplicaSet
                     │
        ┌────────────┼────────────┐
        │            │            │
      Pod 1        Pod 2        Pod 3
```

If you want to:

* Update the application version
* Scale the application
* Roll back to an older version
* Recover failed Pods

A Deployment performs these tasks automatically.

---

# Features of Deployments

Deployments provide several powerful features:

* Manage ReplicaSets automatically.
* Maintain the desired number of Pods.
* Rolling Updates.
* Rollbacks.
* Easy scaling.
* Self-healing.
* Version history.

---

# Example Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: web-deployment

spec:
  replicas: 2

  selector:
    matchLabels:
      app: web

  template:
    metadata:
      labels:
        app: web

    spec:
      containers:
      - name: nginx
        image: nginx:1.26.2
        ports:
        - containerPort: 80
```

Create the Deployment:

```bash
kubectl apply -f deployment.yaml
```

---

# Understanding the YAML

## replicas

Specifies the desired number of Pods.

```yaml
replicas: 2
```

---

## selector

Specifies which Pods belong to the Deployment.

```yaml
selector:
  matchLabels:
    app: web
```

The selector must match the labels defined in the Pod template.

---

## template

Defines the Pod template that Kubernetes uses to create new Pods.

```yaml
template:
  metadata:
    labels:
      app: web
```

Every Pod created by the Deployment will have these labels.

---

## containers

Defines the containers that will run inside the Pod.

```yaml
containers:
- name: nginx
  image: nginx:1.26.2
```

---

# Common Deployment Commands

## Create a Deployment

```bash
kubectl apply -f deployment.yaml
```

---

## List Deployments

```bash
kubectl get deployments
```

or

```bash
kubectl get deploy
```

Example output:

```text
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
web-deployment     2/2     2            2           5m
```

---

## View Detailed Information

```bash
kubectl describe deployment web-deployment
```

Displays:

* Labels
* Selectors
* ReplicaSets
* Events
* Update strategy

---

## List ReplicaSets Created by the Deployment

```bash
kubectl get rs
```

---

## List Pods Created by the Deployment

```bash
kubectl get pods
```

---

# Scaling a Deployment

Increase the number of replicas.

```bash
kubectl scale deployment web-deployment --replicas=5
```

Decrease the number of replicas.

```bash
kubectl scale deployment web-deployment --replicas=2
```

---

# Update the Application

Suppose you want to update the application from **nginx:1.26.2** to **nginx:1.27**.

```bash
kubectl set image deployment/web-deployment nginx=nginx:1.27
```

Kubernetes performs a **Rolling Update** by:

* Creating new Pods.
* Gradually removing old Pods.
* Ensuring the application remains available during the update.

---

# Check Rollout Status

```bash
kubectl rollout status deployment web-deployment
```

Displays the progress of the Deployment update.

---

# View Rollout History

```bash
kubectl rollout history deployment web-deployment
```

Shows all revisions of the Deployment.

---

# Roll Back to the Previous Version

If the latest deployment has issues, roll back to the previous version.

```bash
kubectl rollout undo deployment web-deployment
```

Rollback to a specific revision.

```bash
kubectl rollout undo deployment web-deployment --to-revision=2
```

---

# Delete a Deployment

```bash
kubectl delete deployment web-deployment
```

Deleting the Deployment also removes the ReplicaSets and Pods created by it.

---

# Deployment vs ReplicaSet

| Feature                    | ReplicaSet | Deployment |
| -------------------------- | ---------- | ---------- |
| Maintains Pod replicas     | ✅ Yes      | ✅ Yes      |
| Creates ReplicaSets        | ❌ No       | ✅ Yes      |
| Rolling Updates            | ❌ No       | ✅ Yes      |
| Rollbacks                  | ❌ No       | ✅ Yes      |
| Revision History           | ❌ No       | ✅ Yes      |
| Recommended for production | ❌ No       | ✅ Yes      |

---

# Deployment Lifecycle

```text
kubectl apply
        │
        ▼
Deployment
        │
        ▼
ReplicaSet
        │
        ▼
Pods
        │
        ▼
Application Running
```

---

# Best Practices

* Use **Deployments** for stateless applications.
* Never create Pods directly in production.
* Always use labels and selectors consistently.
* Keep the Deployment YAML in version control (Git).
* Use Rolling Updates instead of deleting and recreating Pods.

---

# Key Takeaways

* A Deployment is the recommended way to deploy applications in Kubernetes.
* It creates and manages ReplicaSets.
* ReplicaSets create and manage Pods.
* Deployments support scaling, rolling updates, rollbacks, and revision history.
* They provide high availability with minimal downtime during updates.

---

# Quick Reference

| Command                                                   | Description                       |
| --------------------------------------------------------- | --------------------------------- |
| `kubectl apply -f deployment.yaml`                        | Create a Deployment               |
| `kubectl get deployments`                                 | List Deployments                  |
| `kubectl describe deployment <name>`                      | Show Deployment details           |
| `kubectl get rs`                                          | List ReplicaSets                  |
| `kubectl get pods`                                        | List Pods                         |
| `kubectl scale deployment <name> --replicas=5`            | Scale the Deployment              |
| `kubectl set image deployment/<name> <container>=<image>` | Update the container image        |
| `kubectl rollout status deployment <name>`                | Check rollout progress            |
| `kubectl rollout history deployment <name>`               | View revision history             |
| `kubectl rollout undo deployment <name>`                  | Roll back to the previous version |
| `kubectl delete deployment <name>`                        | Delete the Deployment             |
