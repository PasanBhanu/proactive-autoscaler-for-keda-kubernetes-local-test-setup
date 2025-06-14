# KEDA

## Install KEDA

helm repo add kedacore https://kedacore.github.io/charts  
helm repo update

kubectl create namespace keda
helm upgrade --install keda kedacore/keda --values keda/values.yaml --namespace keda

## Uninstall KEDA

helm uninstall keda --namespace keda
kubectl delete namespace keda

# Prometheus

## Install Prometheus

kubectl create namespace monitoring
helm upgrade --install prometheus oci://registry-1.docker.io/bitnamicharts/kube-prometheus --values prometheus/values.yaml --namespace monitoring

## Install Monitoring Probe
kubectl apply -f test-app/monitoring.yaml -n monitoring

## Install Custom Scaler
kubectl apply -f keda/deployment.yaml -n keda

## Create Autoscaler Target
kubectl apply -f keda/keda-lsfb-scaler.yaml

# Grafana

## Install Grafana

helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

helm install grafana grafana/grafana --values grafana/values.yaml --namespace monitoring

# Testing

## Port Forwarding for Testing
Prometheus: kubectl port-forward service/prometheus-kube-prometheus-prometheus 9090:9090 -n monitoring
API: 
Grafana: kubectl port-forward service/grafana 3000:80 -n monitoring

## Grafana Dashboard IDs
4701
https://github.com/kedacore/keda/blob/main/config/grafana/keda-dashboard.json