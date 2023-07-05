## Based
https://github.com/GoogleCloudPlatform/microservices-demo
https://github.com/GoogleCloudPlatform/microservices-demo/blob/main/release/kubernetes-manifests.yaml

https://github.com/didier-durand/microservices-on-cloud-kubernetes

## Install Remote
```
kubectl create ns store
kustomize build github.com/paurosello/demoappskustomize//kustomize-store | kubectl apply -n store -f -
```
## Install Local
```
kubectl create ns store
kustomize build kustomize-gapps/ | kubectl apply -n store -f -
```

## Check Load

### Total requests
kubectl logs -l app=loadgenerator -c main | grep Aggregated | awk '{print $2}'

### Errors
kubectl logs -l app=loadgenerator -c main | grep Aggregated | awk '{print $3}' | sed "s/[(][^)]*[)]//g"

### Script check

```
while true
do
    kubectl get nodes >> /tmp/error_logs
    kubectl logs -l app=loadgenerator -c main | grep Aggregated | awk '{print $2} {print $3}' >> /tmp/error_logs
    cat /tmp/error_logs
    sleep 10
done
```

### Run loadgenerator locally
docker run -it -e FRONTEND_ADDR=a1da6f53586e94f3f96d72c896163a49-59542706.eu-central-1.elb.amazonaws.com -e USERS=10 gcr.io/google-samples/microservices-demo/loadgenerator:v0.5.2
