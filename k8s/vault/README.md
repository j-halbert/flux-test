# Manual Vault Deployment 

Using the files here as a template, the following should be a working deployment.  
Edit the deployment.yaml to provide a more appropriate mount path, then just issue
the following commands from this directory.

```
kubectl create configmap vault --from-file=./config.json
kubectl apply -f service.yaml
kubectl apply -f deployment.yaml
```
