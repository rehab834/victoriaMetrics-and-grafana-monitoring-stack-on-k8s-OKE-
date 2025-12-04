## Vicotria-metrics
- you will find docs in same folder under `victoria-metrics.md`

## otel collector
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
helm repo update

### read values.yaml  ( you need to take a look at *.yaml file inside helm chart folder (you will find) )
helm install otel-collector open-telemetry/opentelemetry-collector -f victoria-values.yaml -n monitoring
helm upgrade -i otel-collector open-telemetry/opentelemetry-collector -f values.yaml 

## tempo
helm repo add grafana https://grafana.github.io/helm-charts
helm install tempo grafana/tempo
### tempo port forward
kubectl port-forward svc/tempo 3100:310
http://tempo.default.svc.cluster.local:3200


## mimir
helm install mimir grafana/mimir-distributed
kubectl port-forward svc/mimir-distributed-mimir-query-frontend 9000:80
### port forward
kubectl port-forward svc/mimir-query-frontend 9000:8080
