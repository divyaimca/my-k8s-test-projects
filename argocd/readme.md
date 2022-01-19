
## STEPS FOR INSTLLING ARGOCD AND ACCESS VIA NGINX INGRESS CONTROLLER

# Install Argocd

kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


## argocd cli

brew install argocd 


## Install nginx controller

helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
  
## check nginx pods

kubectl get pods --namespace=ingress-nginx

## wait for nginx pod creation complete 
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=120s
  
## Local testing with a dummy deployments

kubectl create deployment demo --image=httpd --port=80

kubectl expose deployment demo

## Expose via ingress

kubectl create ingress demo-localhost --class=nginx \
  --rule=demo.localdev.me/*=demo:80
  
## Access the above domain url : demo.localdev.me

## Get the argocd admin secret password

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

## Port forward

kubectl port-forward svc/argocd-server -n argocd 8080:443

## Update password

# Login

argocd login 127.0.0.1:8080

# Update

argocd account update-password

## Apply the ingress manifest for argocd

kubectl apply -f https://github.com/divyaimca/my-k8s-test-projects/blob/main/argocd/argocd_local_ingress.yaml

## Access argocd by using the hostname defined in the above manifest




  
  

