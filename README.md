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

## Step 7: Test accessing your deployed app

### Open a new shell session amd get all information to find the service (e.g we will use the line with `service/myhelmapp`)
```bash
kubectl get all -n dev
```

### Setup port forwarding to your app
```bash
kubectl port-forward service/myhelmapp -n dev 8889:80
```

- Visit `http://localhost:8889`

## Step 8: Setup a test API via the UI 
- To add an app, follow the guide found in the link below:
   - Creating Apps via GUI: https://argo-cd.readthedocs.io/en/stable/getting_started/#creating-apps-via-ui

## Step 9: Test deploying out the FastAPI Star Wars API Wrapper
- Using the helm chart repo below, add this into the GUI and then select the `fastapi-starwars-api-helm-chart` path
    - Helm chart repo: https://github.com/bmorri13/starwars_api_helm_charts

### In a seperate browser window, test going to the below address:
- Visit `http://<ip of your cluster or instance>:30005`
> NOTE: This chart was tested on a local k3s cluster meaning the node port forwarding may need to be updated to account for a different cluster type

 
## Additional Notes
- This guide was put together by using:
  - ArgoCD Steps: [https://argo-cd.readthedocs.io/en/stable/getting_started/](https://argo-cd.readthedocs.io/en/stable/getting_started/)
  - Sample Helm Chart: [https://github.com/devopsjourney1/argo-examples](https://github.com/devopsjourney1/argo-examples)
  - Sample Helm Chart API's: [https://github.com/bmorri13/fastapi_multistage_starwars_api_wrapper](https://github.com/bmorri13/fastapi_multistage_starwars_api_wrapper)

## To Do Items
- Test out creating apps via CLI: https://argo-cd.readthedocs.io/en/stable/getting_started/#creating-apps-via-cli
