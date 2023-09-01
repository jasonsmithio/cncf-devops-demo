# Tekton on Kubernetes

## Install [//]: #<https://tekton.dev/docs/installation/pipelines/>

### For Staging Google Kubernetes Edition (GKE)

For this demo, I am using GKE but you can use any Kubernetes cluster.

### Setting up GKE

1. Assuming you haven't already enabled the GKE APIs, you will need to do this first

```bash
gcloud services enable container.googleapis.com
```

2. Create a cluster with Workload Identity enabled

```bash
gcloud container clusters create tekton-cluster \
  --num-nodes=<nodes> \
  --region=<location> \
  --workload-pool=<project-id>.svc.id.goog
  ```

  3. IF PRIVATE CLUSTER

```bash
gcloud compute firewall-rules update <firewall_rule_name> --allow tcp:8443
```

4. IF USING AUTOPILOT

Open port 8443 in firewall rules

```bash
gcloud compute firewall-rules update <firewall_rule_name> --allow tcp:8443
```

Then disable the affinity assistant

```bash
kubectl patch cm feature-flags -n tekton-pipelines \
  -p '{"data":{"disable-affinity-assistant":"true"}}'
```

### Installing [Tekton Pipelines](https://tekton.dev/docs/pipelines/pipelines/)

1. For our use case, we will use the latest official release

```bash
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
```

2. Monitor the installation

```bash
kubectl get pods --namespace tekton-pipelines --watch
```

When all components show 1/1 under the READY column, the installation is complete. Hit Ctrl + C to stop monitoring.

## Installing [Tekton Triggers](https://tekton.dev/docs/triggers/)

1. Install the latest official release

```bash
kubectl apply --filename \
https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml
kubectl apply --filename \
https://storage.googleapis.com/tekton-releases/triggers/latest/interceptors.yaml
```

2. Monitor the installation

```bash
kubectl get pods --namespace tekton-pipelines --watch
```

Just like before, when all components show 1/1 under the READY column, the installation is complete. Hit Ctrl + C to stop monitoring.

### Installing [Tekton Dashboard](https://tekton.dev/docs/dashboard)

1. Install the latest Tekton Dashboard

```bash
kubectl apply --filename https://storage.googleapis.com/tekton-releases/dashboard/latest/release.yaml
```
2. Monitor the Installation

```bash
kubectl get pods --namespace tekton-pipelines --watch
```

Just like before, when all components show 1/1 under the READY column, the installation is complete. Hit Ctrl + C to stop monitoring.
