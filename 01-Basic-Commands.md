# Basic Commands

This document contains commonly used Kubernetes commands and their purpose.

---

## 1. `kubectl api-resources`

### Purpose

Lists all Kubernetes resource types that the Kubernetes API Server knows how to manage.

```bash
kubectl api-resources
```

> **Note**
>
> This command **does not** show the resources currently running in the cluster. Instead, it displays the resource types supported by the Kubernetes API Server.

### Key Points

* Lists all supported Kubernetes resources.
* Shows whether a resource is **Namespaced** or **Cluster-scoped**.
* Useful for discovering available Kubernetes object types.

---

## 2. `kubectl explain`

### Purpose

Displays detailed documentation for any Kubernetes resource.

```bash
kubectl explain <resource-name>
```

### Example

```bash
kubectl explain pod
```

To display all supported fields recursively:

```bash
kubectl explain pod --recursive
```

Another example:

```bash
kubectl explain deployment --recursive
```

### Useful For

* Learning Kubernetes YAML fields
* Understanding supported properties
* Viewing nested object hierarchy

---

## 3. `kubectl config view`

### Purpose

Displays the Kubernetes configuration (`kubeconfig`) currently used by `kubectl`.

```bash
kubectl config view
```

### Displays

* Cluster information
* User information
* Certificates
* Contexts
* Current configuration

---

## 4. `kubectl config current-context`

### Purpose

Displays the Kubernetes context currently being used by `kubectl`.

```bash
kubectl config current-context
```

### Example

```text
kubernetes-admin@yadavpawan.jiyalal
```

Where:

* **kubernetes-admin** → Kubernetes user
* **yadavpawan.jiyalal** → Kubernetes cluster

This command is useful when working with multiple clusters to verify the active context.

---

## 5. `kubectl config get-users`

### Purpose

Lists all users configured in the local Kubernetes configuration.

```bash
kubectl config get-users
```

---

## 6. `kubectl config get-contexts`

### Purpose

Displays all Kubernetes contexts configured in the kubeconfig file.

A context is a combination of:

* Cluster
* User
* Namespace (optional)

```bash
kubectl config get-contexts
```

Useful when working with multiple Kubernetes clusters such as Development, QA, and Production.

---

## 7. `kubectl config use-context`

### Purpose

Switches the active Kubernetes context.

```bash
kubectl config use-context <context-name>
```

### Example

```bash
kubectl config use-context production-cluster
```

Verify the active context:

```bash
kubectl config current-context
```

---

## Quick Reference

| Command                          | Description                                                      |
| -------------------------------- | ---------------------------------------------------------------- |
| `kubectl api-resources`          | Lists all Kubernetes resource types supported by the API Server. |
| `kubectl explain`                | Displays documentation for Kubernetes resources.                 |
| `kubectl config view`            | Shows the current kubeconfig configuration.                      |
| `kubectl config current-context` | Displays the active Kubernetes context.                          |
| `kubectl config get-users`       | Lists all configured Kubernetes users.                           |
| `kubectl config get-contexts`    | Lists all available Kubernetes contexts.                         |
| `kubectl config use-context`     | Switches to a different Kubernetes context.                      |
