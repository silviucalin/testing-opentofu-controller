export GITHUB_USER=silviucalin
export GITHUB_REPO=testing-opentofu-controller

cat <<EOF | kubectl apply -f -
---
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: test
  namespace: flux-system
spec:
  interval: 30s
  url: https://github.com/${GITHUB_USER}/${GITHUB_REPO}
  ref:
    branch: main
---
apiVersion: infra.contrib.fluxcd.io/v1alpha2
kind: Terraform
metadata:
  name: branch-planner-demo
  namespace: flux-system
spec:
  approvePlan: auto
  path: ./
  interval: 1m
  sourceRef:
    kind: GitRepository
    name: branch-planner-demo
    namespace: flux-system
EOF