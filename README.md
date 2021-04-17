# ops-flux

## Sealed secrets

```bash
kubeseal --fetch-cert --controller-name=sealed-secrets --controller-namespace=kube-system > pub-sealed-secrets.pem
```
