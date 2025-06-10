# Install KEDA

kubectl create namespace keda
helm upgrade --install keda kedacore/keda --values keda/values.yaml --namespace keda

# Install Prometheus

# Install Custom Scaler
kubectl apply -f keda/deployment.yaml -n keda

# Create Autoscaler Target
kubectl apply -f keda/keda-lsfb-scaler.yaml