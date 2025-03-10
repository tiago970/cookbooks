# Backstage

<!--
https://github.com/backstage/backstage/tree/master/contrib/chart/backstage
https://nocomplexity.com/spotify-backstage/
https://github.com/backstage/backstage/tree/master/contrib/chart/backstage
-->

## Links

- [Code Repository](https://github.com/backstage/backstage)
- [Main Website](https://backstage.io/)

## References

- [Getting started with Backstage](https://github.com/spotify/backstage/blob/master/docs/getting-started/README.md)

## Helm

### References

- [Values](https://github.com/deliveryhero/helm-charts/tree/master/stable/backstage#values)

### Repository

```sh
helm repo add deliveryhero 'https://charts.deliveryhero.io'
helm repo update
```

### Install

```sh
#
kubectl create ns backstage

#
export INGRESS_HOST='127.0.0.1'

#
helm install backstage deliveryhero/backstage \
  --namespace backstage \
  --version 0.1.12 \
  -f <(cat << EOF
appConfig:
  app:
    baseUrl: http://backstage.${INGRESS_HOST}.nip.io
  backend:
    baseUrl: http://backstage.${INGRESS_HOST}.nip.io
    cors:
      origin: http://backstage.${INGRESS_HOST}.nip.io
  techdocs:
    storageUrl: http://backstage.${INGRESS_HOST}.nip.io/api/techdocs/static/docs
    requestUrl: http://backstage.${INGRESS_HOST}.nip.io/api/techdocs
  lighthouse:
    baseUrl: http://backstage.${INGRESS_HOST}.nip.io/lighthouse-api
EOF
)
```

### Status

```sh
kubectl rollout status deploy/backstage-backend \
  -n backstage
```

### Logs

```sh
kubectl logs \
  -l 'app.kubernetes.io/instance=backstage' \
  -n backstage \
  -f
```

### Delete

```sh
helm uninstall backstage \
  -n backstage

kubectl delete ns backstage \
  --grace-period=0 \
  --force
```
