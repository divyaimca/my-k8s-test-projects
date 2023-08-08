#Commands to generate the manifests
kubectl create deployment nginx-deployment --image=nginx --replicas=2 --port=80 --dry-run=client -o yaml > nginx-deployment.yaml
kubectl create service clusterip  nginx-deployment --tcp=80:80 --dry-run=client -o yaml > nginx-deployment-svc.yaml
kubectl create ingress nginx-deployment --class=nginx --rule=nginx.k8s.local/=nginx-deployment:80 --dry-run=client -o yaml > nginx-deployment-ingress.yaml

#Package manifests in kustomization.yaml and modify the imagee as needed
Add all 3 manifests in kustomization resources

#Apply
kubectl apply -f kustomization.yaml

