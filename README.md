# prometheus-grafana-helm-installation
kubectl create ns apm
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts 
helm repo update 
helm search repo prometheus
helm install prometheus prometheus-community/prometheus -n apm
# ingress for prometheus
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: prometheus-ingress
spec:
  ingressClassName: nginx
  rules:
  - host: prometheus.smartuniversaldevops.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: prometheus-server
            port:
              number: 80
# grafana installation
  helm repo add grafana https://grafana.github.io/helm-charts 
  helm repo update
  helm search repo grafana
  helm install grafana grafana/grafana -n apm
# ingress for grafana
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: grafana-ingress
spec:
  ingressClassName: nginx
  rules:
  - host: grafana.smartuniversaldevops.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: grafana
            port:
              number: 80
