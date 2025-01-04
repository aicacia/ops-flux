# flux

- install flux https://fluxcd.io/flux/installation/
- uninstall `flux uninstall --namespace=flux-system`
- any changes should pass [scripts/validate.sh](scripts/validate.sh)

## Local

- ```bash
  flux bootstrap git \
  --url=ssh://git@github.com/aicacia/ops-flux.git \
  --private-key-file=$HOME/.ssh/id_rsa \
  --branch=main \
  --path=clusters/production
  ```
- create token admin-user token `kubectl -n kube-public create token admin-user`
- kubernetes dashboard `https://kubernetes-dashboard.docker.internal`
- if you have to uninstall and reinstall flux, somethings the listener on \*:80 for the nginx ingress fails to die
  `sudo lsof -nP -i4TCP:80 | grep LISTEN`
  `kill -9 $PID`

## Sealed secrets

```bash
cat secrets/p2p-api-env-secret.yaml | kubeseal --controller-namespace kube-system --controller-name sealed-secrets --format yaml > apps/production/configs/p2p-api-env-secret.yaml
```

Backup `kubectl -n kube-system get secret -l sealedsecrets.bitnami.com/sealed-secrets-key=active -o yaml > secrets/$(date '+%Y-%m-%d').yml`

## Docker creds

```bash
kubectl create secret generic registry \
    --namespace api \
    --from-file=.dockerconfigjson=secrets/ghcr.json \
    --type=kubernetes.io/dockerconfigjson \
    --dry-run=client \
    -o yaml
```
