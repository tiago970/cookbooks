# Actions YAML Validator

## Workflow

```yaml
name: YAML Validator

on:
  pull_request:
    paths:
    - .github/**.yaml
    - k8s/**.yaml
  push:
    branches:
    - main
    paths:
    - .github/**.yaml
    - k8s/**.yaml

jobs:
  validation:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Code
      uses: actions/checkout@v2

    - name: Linting YAML Files
      uses: ibiqlik/action-yamllint@v2

    - name: Validate Kubernetes Manifests
      uses: stefanprodan/kube-tools@v1.6.0
      with:
        kustomize: 3.8.8
        kubeval: v0.16.1
        command: |
          echo 'Run kubeval'
          for ENV in `find ./.k8s/overlays -type d | awk 'NR > 1'`; do
            echo "Validating: $ENV"
            kustomize build "$ENV" | \
              kubeval --skip-kinds 'ExternalSecret,Ingress'
          done
```
