## Overview

Monitoring Kubernetes clusters is crucial for maintaining the health and performance of applications. Prometheus is an open-source monitoring tool that collects and stores metrics, while Grafana is a data visualization tool that displays these metrics in customizable dashboards. Together, they provide a powerful solution for cluster monitoring.

** Prerequisites

- A running Kubernetes cluster (local or cloud-based).

- kubectl command-line tool configured to interact with your cluster.

- Helm package manager installed.

### Minikube would be used as K8s cluster <br><br>
** Start Minikube but ensure Docker is up <br>

```minikube start --driver=docker``` <br>

<img width="996" height="490" alt="image" src="https://github.com/user-attachments/assets/b06857fd-2488-4a0e-889a-3b6ca7e9e521" /><br><br>

<img width="1908" height="480" alt="image" src="https://github.com/user-attachments/assets/7b9c146c-5bb3-44aa-accc-f6696d8f0246" /><br><br>


### Step-by-Step Guide

1. Create a Namespace for Monitoring Tools

It’s a good practice to isolate monitoring tools in their namespace.

```kubectl create namespace monitoring``` <br><br>
<img width="1170" height="382" alt="image" src="https://github.com/user-attachments/assets/878db1a6-a6d2-4a64-835d-c46119311da5" /><br><br>

2. Install Prometheus Using Helm

Add the Prometheus Helm repository and update:

```helm repo add prometheus-community https://prometheus-community.github.io/helm-charts```<br> 
```helm repo update ```<br><br>
<img width="1832" height="314" alt="image" src="https://github.com/user-attachments/assets/8bd40cef-cc6a-4d95-9780-9c0c434f2743" /><br><br>

3. Install Prometheus within the monitoring namespace <br>
<img width="2820" height="1550" alt="image" src="https://github.com/user-attachments/assets/699b2503-b108-412d-a164-d5830645973e" /><br><br>

Verify that Prometheus is running:

```kubectl get pods -n monitoring```<br> 
<img width="1534" height="296" alt="image" src="https://github.com/user-attachments/assets/ac7dbe1a-aacd-4c87-a319-189127c7eb71" /> <br><br>

4. Access the Prometheus Dashboard

Forward the Prometheus server port to access it locally:<br>

``kubectl port-forward -n monitoring deploy/prometheus-server 9090``<br>

<img width="1680" height="138" alt="image" src="https://github.com/user-attachments/assets/301744eb-e937-44bd-a5c3-285c37c7ad3b" /> <br><br>

Prometheus dashboard is up and running.<br>

Open your browser and navigate to http://localhost:9090 to view the Prometheus UI.

<img width="1471" height="817" alt="image" src="https://github.com/user-attachments/assets/e3aef8da-581f-42c0-9846-ece544c4eac2" /><br><br>

5. Install Grafana Using Helm

Add the Grafana Helm repository and update:

```helm repo add grafana https://grafana.github.io/helm-charts```<br> 
```helm repo update```<br>

<img width="1622" height="412" alt="image" src="https://github.com/user-attachments/assets/6f55ce12-e3ed-4456-82cf-5aa6b6f8c021" /><br><br>

Install Grafana in the monitoring namespace:<br> 

```helm install grafana grafana/grafana --namespace monitoring```<br> 

<img width="2696" height="846" alt="image" src="https://github.com/user-attachments/assets/f951bdcd-c77e-444c-a12c-f84968c59b97" /><br><br>

<img width="1718" height="330" alt="image" src="https://github.com/user-attachments/assets/e5d1b439-bb8f-4067-8b09-c93a75205d20" /><br><br>


Retrieve admin login details for Grafana <br>

```kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo```<br> 

<img width="2182" height="40" alt="image" src="https://github.com/user-attachments/assets/e7d14d1a-059c-4b54-9e0f-a5603079e8ce" />

Forward the Grafana service port:<br>

```kubectl port-forward -n monitoring service/grafana 3000:80```<br>
Access Grafana at http://localhost:3000 and log in with username admin and the retrieved password.<br><br>
<img width="2940" height="1760" alt="image" src="https://github.com/user-attachments/assets/8e5a9b03-0231-4b95-8e95-78bdead08bc2" /><br><br>
<img width="2932" height="1762" alt="image" src="https://github.com/user-attachments/assets/2092dd15-9576-4825-a317-f486f4d1f130" /><br><br>

Persist Grafana state via the command line <br>

helm upgrade grafana grafana/grafana --namespace monitoring \ <br>
  --set persistence.enabled=true \ <br>
  --set persistence.size=1Gi \ <br>
  --set initChownData.enabled=false \ <br>
  --set adminPassword=generated-password <br>

6. Configure Prometheus as a Data Source in Grafana

In the Grafana UI:

- Navigate to Configuration > Data Sources.

- Click Add data source.

- Select Prometheus from the list.

- Set the URL to http://prometheus-server.monitoring.svc.cluster.local:80.

- Click Save & Test to verify the connection.<br>

<img width="1914" height="526" alt="image" src="https://github.com/user-attachments/assets/b4bfbf74-4523-4aff-80d7-8b4481286d05" /><br><br>

7. Import a Kubernetes Dashboard into Grafana

Grafana offers pre-built dashboards for Kubernetes monitoring.

- Go to Create > Import in Grafana.

- Enter a dashboard ID from Grafana’s dashboard repository (e.g., 6417 for Kubernetes cluster monitoring).

- Click Load.

- Select the Prometheus data source you added earlier.

- Click Import.

Explore the Metrics and Dashboards

With the dashboard imported, you can now:

Monitor real-time metrics such as CPU usage, memory consumption, and network traffic.

View pod statuses and cluster health.

Customize panels and visualizations according to your needs.

<img width="2916" height="1838" alt="image" src="https://github.com/user-attachments/assets/e6527703-5aa4-4ba5-b75b-d1753d920643" /><br><br>


***Additional Tips***

Persistent Storage: For production environments, configure persistent volumes for Prometheus and Grafana to retain data after pod restarts.

Set Up Alerting: Configure Prometheus alerting rules and integrate with notification channels like Slack or email to receive alerts on critical issues.``https://grafana.com/tutorials/alerting-get-started/``

Secure Access: Implement authentication and secure connections (TLS/SSL) for accessing Prometheus and Grafana dashboards.

Scaling: Modify resource limits and replica counts in the Helm charts to scale the monitoring stack according to your cluster size.

***Benefits of This Setup***

Centralized Monitoring: Offers a single point to observe the entire Kubernetes cluster.

Customizable Dashboards: Grafana allows for extensive customization to focus on the metrics that matter most.

Proactive Issue Detection: Early detection of anomalies helps in quicker resolution and less downtime.









