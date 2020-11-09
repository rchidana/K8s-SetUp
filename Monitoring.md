Install Helm3

```
>curl https://baltocdn.com/helm/signing.asc | sudo apt-key add - <br>
>sudo apt-get install apt-transport-https --yes <br>
>echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list <br>
>sudo apt-get update<br>
>sudo apt-get install helm<br>

>helm version
```


Helm3 Repo URLs:

prometheus-community    https://prometheus-community.github.io/helm-charts<br>
grafana                 https://grafana.github.io/helm-charts<br>

```
>helm repo add prometheus-community https://prometheus-community.github.io/helm-charts <br>
>helm search repo prometheus<br>

>helm install prometheus prometheus-community/prometheus
	
>kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-svc<br>

>helm repo add grafana https://grafana.github.io/helm-charts <br>
>helm install grafana grafana/grafana <br>

```
Copy the command that is required to decode the 'admin' password<br>

```
>kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-svc

>minikube service grafana-svc

```

Import Grafana Dashboard 6417
