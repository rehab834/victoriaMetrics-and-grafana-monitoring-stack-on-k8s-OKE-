# victoriaMetrics-and-grafana-monitoring-stack-on-k8s-OKE-
# 🚀 VictoriaMetrics + Grafana Monitoring Stack on Kubernetes (OKE)

This guide shows how to install a lightweight, production-ready Kubernetes monitoring stack using [VictoriaMetrics](https://victoriametrics.com/) and [Grafana](https://grafana.com/) with Helm. It includes:
- `vmsingle`: Time-series database
- `vmagent`: Scraping engine (like Prometheus)
- `Grafana`: Dashboard and visualization
- `node-exporter` & `kube-state-metrics`: Kubernetes metrics

---

## 📦 Prerequisites

- A running Kubernetes cluster (e.g., OKE)
- `kubectl` configured and authenticated
- `helm` installed and configured

---

## 🛠️ Installation Steps

1. **Add VictoriaMetrics Helm Repository**

```bash
helm repo add victoria-metrics https://victoriametrics.github.io/helm-charts/
helm repo update
```

2. **Install the Monitoring Stack**

```bash
helm install vms victoria-metrics/victoria-metrics-k8s-stack \
  --namespace monitoring \
  --create-namespace \
  --set vmsingle.enabled=true \
  --set vmagent.enabled=true \
  --set grafana.enabled=true \
  --set prometheus-node-exporter.enabled=true \
  --set kube-state-metrics.enabled=true
```

---

## 🔑 Accessing Grafana

### 1. **Port Forward Grafana Service**

```bash
kubectl port-forward svc/vms-grafana -n monitoring 3000:80
```

Now open your browser and go to:  
👉 [http://localhost:3000](http://localhost:3000)

---

### 2. **Retrieve Grafana Admin Credentials*

---

## 📊 Dashboards and Metrics

Once logged in to Grafana:
- VictoriaMetrics will already be configured as a Prometheus-compatible data source.
- Default dashboards will be available for:
  - Node metrics
  - Cluster health (via kube-state-metrics)
  - VictoriaMetrics performance

---

## 🔄 Upgrade or Reconfigure Later

To update Grafana admin password or any other config:

```bash
helm upgrade vms victoria-metrics/victoria-metrics-k8s-stack \
  --namespace <your-ns> \
  --set grafana.adminPassword='your-new-password' \
  --reuse-values
```

---

## ✅ Cleanup (Uninstall)

```bash
helm uninstall vms -n monitoring
kubectl delete namespace monitoring
```

---

## 🧠 Notes

- All components run with minimal resource overhead.
- Works well in production and test environments.
- You can integrate external alerting, long-term storage, and Ingress for more advanced setups.

---
