Install Helm3

>curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
>sudo apt-get install apt-transport-https --yes
>echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
>sudo apt-get update
>sudo apt-get install helm

>helm version

Helm3 Repo URLs:

prometheus-community    https://prometheus-community.github.io/helm-charts
grafana                 https://grafana.github.io/helm-charts


>helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
>helm search repo prometheus

>helm install prometheus prometheus-community/prometheus
	
>kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-svc

>helm repo add grafana https://grafana.github.io/helm-charts
>helm install grafana grafana/grafana

Copy the command that is required to decode the 'admin' password

>kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-svc

>minikube service grafana-svc

Import Grafana Dashboard 6417
