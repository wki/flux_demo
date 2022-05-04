# INFO

# BOOTSTRAP

* returns OK
  flux check --pre

* see: https://github.com/fluxcd/flux2/discussions/2114

* works, pods running
  flux install \
  --version=latest \
  --components=source-controller,kustomize-controller,helm-controller,notification-controller \
  --namespace=flux-system \
  --network-policy=true \
  --log-level=info

* works, deploy key has to be added to github
  flux create source git flux-system \
  --git-implementation=go-git \
  --url=ssh://git@github.com/wki/flux_demo.git \
  --ssh-key-algorithm=rsa \
  --branch=main \
  --interval=5m \
  --timeout=5m

* untested:
  flux create kustomization flux-system \
--source=flux-system \
--path="./cluster" \
--prune=true \
--interval=5m


# create sample helm thingy

flux create source helm traefik --url https://helm.traefik.io/traefik --namespace traefik
flux create helmrelease my-traefik --chart traefik \
  --source HelmRepository/traefik \
  --chart-version 9.18.2 \
  --namespace traefik
