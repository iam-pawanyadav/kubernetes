Basic Commands

This section covers the commonly used Kubernetes commands.

kubectl api-resources
Purpose

Lists every Kubernetes resource type that the API Server knows how to manage.

kubectl api-resources
Notes
Shows all supported Kubernetes resources.
Displays whether the resource is:
Namespaced
Cluster Scoped
Does not show the resources currently running in the cluster.
Only displays the available resource types.
kubectl explain

Displays complete documentation for any Kubernetes resource.

kubectl explain pod

To recursively display all supported fields:

kubectl explain pod --recursive

Example:

kubectl explain deployment --recursive

Useful for:

Learning YAML fields
Finding supported properties
Understanding nested objects
kubectl config view

Displays the kubeconfig used by kubectl.

kubectl config view

Shows:

Cluster information
User information
Certificates
Current context
kubectl config current-context

Displays the current Kubernetes context.

kubectl config current-context

Example

kubernetes-admin@yadavpawan.jiyalal

Where

kubernetes-admin → User
yadavpawan.jiyalal → Cluster
kubectl config get-users

Lists every configured Kubernetes user.

kubectl config get-users
kubectl config get-contexts

Displays all configured contexts.

kubectl config get-contexts

Useful when working with multiple Kubernetes clusters.

kubectl config use-context

Switch to another Kubernetes context.

kubectl config use-context <context-name>

Example

kubectl config use-context production-cluster
