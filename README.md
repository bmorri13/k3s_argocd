# k3s_argocd
- This guide outlines a starter of how to use ArgoCD

## Prerequisites
- Ensure you have a working cluster, for the below example it was tested on `k3s`
- Ensure you have `kubectl` installed and configured to interact with your Kubernetes cluster.
- Ensure you have a namespace named `dev` created
    - E.g. Run `kubectl create ns dev`
 
## Step 1: Create the ArgoCD namespace

### Run Command:
```bash
kubectl create ns argocd
```

## Step 2: Install the ArgoCD Manifest

### Run Command:
```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Step 3: Get the ArgoCD initial secret

### Run Command:
```bash
kubectl get secrets -n argocd
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

## Step 4: Create a dev namespace to deploy your app into

### Run Command:
```bash
kubectl create ns dev
```

## Step 5: Set up port forwarding to access your argocd UI

### Run Command:
```bash
kubectl port-forward svc/argocd-server -n argocd 8888:443
```

- Visit `http://localhost:8888`

## Step 6: Setup your app in the GUI 
- To add an app, follow the guide found in the link below:
   - Creating Apps via GUI: https://argo-cd.readthedocs.io/en/stable/getting_started/#creating-apps-via-ui


 
## Additional Notes
- This guide was put together by using:
  - ArgoCD Steps: [https://argo-cd.readthedocs.io/en/stable/getting_started/](https://argo-cd.readthedocs.io/en/stable/getting_started/)
  - Sample Helm Chart: [https://github.com/devopsjourney1/argo-examples](https://github.com/devopsjourney1/argo-examples)

## To Do Items
- Test out creating apps via CLI: https://argo-cd.readthedocs.io/en/stable/getting_started/#creating-apps-via-cli
