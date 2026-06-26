# Replication Controllers

A **Replication Controller (RC)** is a Kubernetes controller that ensures a specified number of Pod replicas are running at all times.

If a Pod crashes or is deleted, the Replication Controller automatically creates a new Pod to maintain the desired number of replicas.

> **Note**
>
> Replication Controllers are considered a **legacy resource**. In modern Kubernetes, **ReplicaSets** (managed by Deployments) are the recommended replacement.

---

## Why Do We Need a Replication Controller?

Suppose you have a single Pod running your application.

```text
Pod (1)
```

If the Pod crashes, your application becomes unavailable.

A Replication Controller continuously monitors the Pods it manages and ensures the desired number of replicas are always running.

```text
Desired Pods = 3

          Replication Controller
                    │
      ┌─────────────┼─────────────┐
      │             │             │
    Pod 1         Pod 2         Pod 3
```

If **Pod 2** crashes, the Replication Controller immediately creates a new Pod to replace it.

---

## Responsibilities

A Replication Controller is responsible for:

* Maintaining the desired number of Pod replicas.
* Automatically recreating failed or deleted Pods.
* Scaling applications up or down.
* Managing Pods using labels.

---

## Example YAML

```yaml
apiVersion: v1
kind: ReplicationController

metadata:
  name: nginx-rc

spec:
  replicas: 3

  selector:
    app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

---

## Understanding the YAML

### `replicas`

Specifies how many Pods should always be running.

```yaml
replicas: 3
```

---

### `selector`

The selector tells the Replication Controller which Pods it should manage.

```yaml
selector:
  app: nginx
```

Only Pods with the matching label are managed.

---

### `template`

Defines the Pod template used whenever Kubernetes creates a new Pod.

```yaml
template:
  metadata:
    labels:
      app: nginx
```

If a Pod fails, Kubernetes creates a new Pod using this template.

---

## Create a Replication Controller

```bash
kubectl apply -f rc.yaml
```

---

## View Replication Controllers

```bash
kubectl get rc
```

Example output:

```text
NAME       DESIRED   CURRENT   READY   AGE
nginx-rc   3         3         3       2m
```

---

## View Detailed Information

```bash
kubectl describe rc nginx-rc
```

Displays:

* Labels
* Selector
* Replica count
* Events
* Managed Pods

---

## List with Additional Information

```bash
kubectl get rc -o wide
```

---

## Scale a Replication Controller

Increase or decrease the number of Pods.

```bash
kubectl scale rc nginx-rc --replicas=5
```

Kubernetes immediately creates additional Pods.

To scale down:

```bash
kubectl scale rc nginx-rc --replicas=2
```

Extra Pods are terminated automatically.

---

## Delete a Replication Controller

```bash
kubectl delete rc nginx-rc
```

By default, deleting the Replication Controller also deletes the Pods it manages.

---

## Limitations of Replication Controllers

Replication Controllers have several limitations compared to ReplicaSets.

* Supports only **equality-based label selectors**.
* Does **not** support **matchExpressions**.
* Rolling updates require manual intervention.
* Rollbacks are not built in.
* Rarely used in modern Kubernetes deployments.

---

## Replication Controller vs ReplicaSet

| Feature                                       | Replication Controller | ReplicaSet |
| --------------------------------------------- | ---------------------- | ---------- |
| Equality-based selectors                      | ✅ Yes                  | ✅ Yes      |
| Set-based selectors (`In`, `NotIn`, `Exists`) | ❌ No                   | ✅ Yes      |
| Used by Deployments                           | ❌ No                   | ✅ Yes      |
| Recommended today                             | ❌ No                   | ✅ Yes      |

---

## When Should You Use It?

Today, Replication Controllers are mainly useful for:

* Learning Kubernetes fundamentals.
* Understanding the evolution of Kubernetes controllers.
* Working with older Kubernetes clusters.

For new applications, use **Deployments**, which automatically create and manage **ReplicaSets**.

---

## Quick Reference

| Command                                | Description                                          |
| -------------------------------------- | ---------------------------------------------------- |
| `kubectl apply -f rc.yaml`             | Create a Replication Controller                      |
| `kubectl get rc`                       | List Replication Controllers                         |
| `kubectl get rc -o wide`               | List Replication Controllers with additional details |
| `kubectl describe rc <name>`           | Show detailed information                            |
| `kubectl scale rc <name> --replicas=5` | Scale the Replication Controller                     |
| `kubectl delete rc <name>`             | Delete the Replication Controller                    |

---

## Key Takeaways

* A Replication Controller ensures the desired number of Pods are always running.
* It automatically recreates Pods if they fail or are deleted.
* It manages Pods using **label selectors**.
* It is a legacy Kubernetes resource.
* Modern Kubernetes uses **ReplicaSets** (through Deployments) instead of Replication Controllers.
