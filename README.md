# kubernetes

## What is kubernetes?

kubernetes (Gree for helmsman of a ship) is a container orchestration tool). Helps make decisions about where and how containers are launched on a server, when to scale up or down an application, and what to do when an app or server stops working. Kubernetes is written in Go and is open source.

`minikube` is a helpful tool for running kubernetes clusters locally, but should not be used in production. You can install on mac with `brew install minikube`

## What are containers?

A technology that bundles the code and configuration required to run an application in one unit. It has advantages over dedicated servers or VMs intended to run an application. It requires less CPU, is more portable, and can spin up/down and scale faster.

## what are kubernetes namespaces?

Help you organize and isolate workloads.

## helpful code:

### kind

- list all `kind` clusters with `kind get clusters`

### minikube

- start the cluster with `minikube start --driver=docker`. 

- Create a new cluster and start it, use `minikube start -p <clustername>`. 

- Show all minikube clusters with `minikube profile list` 

- Switch to a different cluster with `minikube profile <clustername>`. 

- Stop the active cluster using `minikube stop`. 

- Delete the active minikube cluster with `minikube delete`. 

### kubectl, kubectx, etc

- `kubectl config current-context` # gets the current environment

- `kubectl config get-clusters` # gets clusters currently defined in the kubeconfig

- `kubectl cluster-info` # get cluster info including ip address and DNS

- `kubectl get namespace <namespace>` # gets the namespace

- `kubectl get namespaces` # get all namespaces

- `kubectl get pods` # get all pods in default namespace

- `kubectl get pods -A` # get all pods regardless of namespace

- `kubectl get pods -n <namespace> -o wide` # get all pods in a namespace with extra info including ip address

- `kubectl logs <pod name> -n <namespace>` get application logs from pod

- `kubectl get services -A` # shows the services running in the cluster 

- `kubectl get nodes` # returns all nodes in a kubernetes cluster. shows node name, status, roles, age, and kubernetes version

- `kubectx` OR `kubectl config get-contexts` # list available kubectl contexts

- `kubectx <context>` OR `kubectl config use-context <context>` # switch to the target context

- `kubectl apply -f <yaml manifest>` # apply a kubernetes manifest to the cluster

- `kubectl get deployments -A` or `kubectl get deployments -n <namespace>`

- `kubectl delete pod <pod name> -n <namespace>` # deletes pod from namespace. If replicas are set to N in manifest, kubernetes will automatically ensure there are always N pods running, even if manual deletion of pod is executed.

- `kubectl describe pod <pod> -n <namespace>` # describe the pod manifest and show event logs

- `kubectl exec -it <pod name> -- /bin/sh` # shell into pod

- `stern -n <namespace> <pod prefix> --tail 0` # stream only new logs from one or more k8 pods
- `kubectl get pod -n <namespace> <pod-name> -o jsonpath='{.spec.serviceAccountName}'` # check gcp service account of pod
- `kubectl get serviceaccount -n <namespace> <gcp service account> -o yaml` # check k8 service account
- ```
  kubectl get events \
    --all-namespaces \
    --sort-by='.lastTimestamp' \
    -o custom-columns="NAMESPACE:.metadata.namespace,NAME:.involvedObject.name,KIND:.involvedObject.kind,REASON:.reason,MESSAGE:.message,LAST_SEEN:.lastTimestamp"
  ```
  - get all operations (not necessarily activity) on a k8 cluster
*Note: a kubectl context is a saved combination of three things: cluster, user, namespace*

### helm

- `helm uninstall <name> -n <namespace>` # for uninstalling a release from a kubernetes cluster. user `--keep-history` if needed

- `helm list -A` # list all releases in a cluster

- `helm list -n <namespace>` # list all releases in a cluster namespace

- `helm show values <chart>` # values.yaml file for the chart

### gcp specific kubernetes management

- `glooud container clusters get-credentials <cluster>` # gets and merges credentials into ~/.kube/config for the cluster. does not work for private clusters.

- `gcloud container fleet memberships list --project=<gcp-project-id>` # list fleet memberships (gke clusters registered to the fleet)

- `gcloud container fleet memberships get-credentials <gke cluster> --project=<gcp project id>` # get credentials via Connect Gateway (works for private clusters without VPC access)
- `gcloud iam service-accounts get-iam-policy <iam email>`
- ```gcloud container operations list \
        --project <gcp project> \
        --filter="targetLink:<cluster>" \
        --format="table(name,operationType,status,statusMessage,startTime,endTime)" \
        --sort-by="~startTime"
  ```
     - Lists control-plane operations (not workload activity).
     - Typical use case: diagnosing why a cluster is unhealthy or unresponsive. GKE operations like node auto-repair, version upgrades, or node pool scaling can lock the control plane and
  cause transient issues. This command lets you see what ran recently and whether anything failed (status=DONE vs ABORTING/FAILED) along with any statusMessage error detail. For kubectl command, see `kubectl get events`


There are two separate identities that need to be linked:

  - Kubernetes Service Account — lives inside your cluster, assigned to the pod
  - GCP Service Account — lives in Google Cloud, has permissions to GCP resources (database, storage, etc.)

Workload Identity is the bridge between them. It says "when a pod uses k8s service account, allow it to act as GCP service account.


*Note: a GKE fleet is a gcp concept for grouping and managing multiple kubernetes clusters together.*
