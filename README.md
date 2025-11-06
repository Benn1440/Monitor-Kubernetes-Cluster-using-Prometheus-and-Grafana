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





