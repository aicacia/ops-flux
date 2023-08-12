# flux

- install flux https://fluxcd.io/flux/installation/
- uninstall `flux uninstall --namespace=flux-system`
- any changes should pass [scripts/validate.sh](scripts/validate.sh)

## Local

- ```bash
  flux bootstrap git \
  --url=ssh://git@github.com/aicacia/ops-flux \
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
kubeseal --fetch-cert --controller-name=sealed-secrets --controller-namespace=kube-public > pub-sealed-secrets.pem
```

## Docker creds

```bash
kubectl create secret generic docker-registry \
    --from-file=.dockerconfigjson=/home/$USER/.docker/config.json \
    --type=kubernetes.io/dockerconfigjson \
    --dry-run=client \
    -o yaml
```
