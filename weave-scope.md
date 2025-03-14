# Weave Scope

## Helm

### References

- [Configuration](https://github.com/helm/charts/tree/master/stable/weave-scope#configuration)

### Install

```sh
kubectl create ns weave-scope
```

```sh
kubectl create secret tls example.tls-secret \
  --cert='/etc/ssl/certs/example/root-ca.crt' \
  --key='/etc/ssl/private/example/root-ca.key' \
  -n weave-scope
```

```sh
helm install weave-scope stable/weave-scope \
  --namespace weave-scope \
  --set weave-scope-frontend.ingress.enabled=true \
  --set weave-scope-frontend.ingress.paths={/} \
  --set weave-scope-frontend.ingress.hosts={scope.${INGRESS_HOST}.nip.io} \
  --set 'weave-scope-frontend.ingress.tls[0].secretName=example.tls-secret' \
  --set 'weave-scope-frontend.ingress.tls[0].hosts={scope.${INGRESS_HOST}.nip.io}'
```

### Status

```sh
kubectl rollout status deploy/weave-scope-frontend-weave-scope -n weave-scope
```

### Delete

```sh
helm uninstall weave-scope -n weave-scope

kubectl delete ns weave-scope --grace-period=0 --force
```
