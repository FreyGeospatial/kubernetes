# kubernetes

## What is kubernetes?

kubernetes (Gree for helmsman of a ship) is a container orchestration tool). Helps make decisions about where and how containers are launched on a server, when to scale up or down an application, and what to do when an app or server stops working. Kubernetes is written in Go and is open source.

## What are containers?

A technology that bundles the code and configuration required to run an application in one unit. It has advantages over dedicated servers or VMs intended to run an application. It requires less CPU, is more portable, and can spin up/down and scale faster.

## running kubernetes locally

minikube is a helpful tool for running kubernetes clusters locally, but should not be used in production. You can install on mac with `brew install minikube` and start the cluster with `minikube start --driver=docker`

## helpful code:

`helm uninstall <name> -n <namespace>` # for uninstalling a release from a kubernetes cluster

`helm list -A` # list all releases in a cluster

`helm list -n <namespace>` # list all releases in a cluster namespace

`kubectl config current-context` # gets the current environment

`kubectl get namespace <namespace>` # gets the namespace

`kubectl get nodes` # returns all nodes in a kubernetes cluster. shows node name, status, roles, age, and kubernetes version

`glooud container clusters get-credentials <cluster>` # gets and merges credentials into ~/.kube/config for the cluster. does not work for private clusters.

`gcloud container fleet memberships list --project=<gcp-project-id>` # list fleet memberships (gke clusters registered to the fleet)

`gcloud container fleet memberships get-credentials <gke cluster> --project=<gcp project id>` # get credentials via Connect Gateway (works for private clusters without VPC access)

`kubectx` # list available kubectl contexts

`kubectx <context>` # switch to the target context


## fleets

a GKE fleet is a gcp concept for grouping and managing multiple kubernetes clusters together.

## context

a kubectl context is a saved combination of three things:
- cluster
- user
- namespace
