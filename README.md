# kubernetes

helpful code:

`helm uninstall <name> -n <namespace>` # for uninstalling a release from a kubernetes cluster

`helm list -A` # list all releases in a cluster

`helm list -n <namespace>` # list all releases in a cluster namespace

`kubectl config current-context` # gets the current environment

`kubectl get namespace <namespace>` # gets the namespace

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
