# `kubectl` command

Get all pods
```shell
kubectl get pods -A
```

Describe pod
```shell
kubectl describe pod $POD_NAME
```

Delete pod
```shell
kubectl delete pod $POD_NAME
```

or by referencing the manifest
```shell
kubectl delete pod -f $MANIFEST.yaml
```

Force delete:
```shell
kubectl delete pod $POD_NAME --grace-period=0 --force
```

## Pod logs

```shell
kubectl logs $POD_NAME
```

To look for errors in the logs of the previous pod that crashed:
```shell
kubectl logs --previous $POD_NAME
```

Logs by container inside a pod
```shell
kubectl logs $POD_NAME -c $CONTAINER_NAME
```

The flag `-f` (`--follow`) 	Specify if the logs should be streamed.

##  Manage context

List existing context
```shell
kubectl config get-contexts
```

Select context
```shell
kubectl config use-context $CONTEXT_NAME
```

Set context namespace
```shell
kubectl config set-context $CONTEXT_NAME --namespace=$NAMESPACE
```
for unset use `--namespace=''`

List supported API resources and abbreviations on the server
```shell
kubectl api-resources -o wide
```

List resources within a helm release:
```shell
kubectl api-resources --verbs=list -o name | xargs -n 1 kubectl get --show-kind -l app.kubernetes.io/instance=${$RELEASE_NAME} --ignore-not-found -o name
```