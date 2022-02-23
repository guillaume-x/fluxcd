# Fluxcd clusters repository

## Flux installation

Fluxcd documentation : 
 * https://fluxcd.io/docs/installation/

Create Github Token with 'repo' and 'admin' right
```bash
export GITHUB_TOKEN=<TOKEN>
```

```bash
# prepare github repo
git clone git@github.com:guillaume-x/fluxcd.git
cd fluxcd
mkdir -p clusters/k3s-azn01/flux-system
cd clusters/k3s-azn01/flux-system
touch gotk-components.yaml gotk-sync.yaml kustomization.yaml
# add kustomization.yaml - see file description underthis section
cd -
# commit & push

# check if all is ok
flux check --pre

# generate ssh key ~/.ssh/id_rsa_fluxcd_k3s-azn01
ssh-keygen -t rsa

# bootstrap the flux
flux bootstrap github  --owner=guillaume-x --repository=fluxcd --branch=main \ 
  --path=clusters/k3s-azn01 --private-key-file=~/.ssh/id_rsa_fluxcd_k3s-azn01 \
  --personal
```

kustomization.yaml
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources: # manifests generated during bootstrap
  - gotk-components.yaml
  - gotk-sync.yaml
```
