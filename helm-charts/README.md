# Helm Charts

## App-of-Apps pattern

1. Create a helm chart as a root application with structure.
```
root-app
  |
  |-- templates
  |     |-- backend.yaml
  |     |-- frontend.yaml
  |     |-- mongodb.yaml
  |     |-- prometheus.yaml // For monitoring
  |     |-- grafana.yaml // For monitoring
  |     |-- ecr-credential.yaml // Only for credential to run ArgoCD Image Updater
  |-- Chart.yaml
  |-- values.yaml
```
- Each yaml file in templates folder will point to sub-chart in `apps` chart below

2. Create other helm chart with multiple sub-charts
    - backend
    - frontend
    - mongodb
```
apps
  |
  |-- Chart.yaml
  |-- values.yaml
  |-- charts
  |     |-- backend
  |     |-- frontend
  |     |-- mongodb
```

## Promotion Environment pattern
- In order to apply Promotion Environment pattern we will create `values.yaml` file for each environment
    - Dev: `values-dev.yaml`
    - Staging: `values-staging.yaml`
    - Production: `values-prod.yaml`

- Run command below to create argocd Application or create on UI instead
- Replace value of `--values` option with any values.yaml file above for each environment

```sh
argocd app create argo-app-name --repo https://github.com/Chau-NH/helm-charts.git --path root-app --dest-server https://kubernetes.default.svc --dest-namespace argocd --values values.yaml
```