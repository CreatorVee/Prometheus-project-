📊 Kubernetes Application Monitoring with Prometheus

---

🛠 Tech Stack


---
📝 Project Overview

The goal of this project was to monitor a Java application, MySQL database, and Nginx Ingress controller deployed on a Kubernetes cluster, and to configure Prometheus to send email alerts when critical conditions occur.

Key objectives:

Deploy Java app with MySQL backend in Kubernetes

Expose the app via Nginx Ingress

Monitor applications with Prometheus and Node/Service exporters

Configure alert rules for critical scenarios

Verify that alerts are delivered via email

---

🚨 Problem

Before this setup:

Application health and performance metrics were not centrally monitored.

Critical issues (e.g., MySQL down, too many connections, high HTTP errors) could go unnoticed.

Manual checks were required, which are time-consuming and error-prone.

---

✅ Solution

This project uses Kubernetes + Prometheus + Helm to create a self-monitoring environment:

Java App & MySQL Deployment

Java app deployed with 3 replicas

MySQL deployed as a StatefulSet with 2 replicas

Application communicates with MySQL database

Ingress Controller

Nginx deployed to route traffic to the Java application

Monitoring & Alerts

Prometheus Operator deployed using Helm

Metrics scraping configured for Java app, MySQL, and Nginx

Alerts configured for:

HTTP 4xx errors on Nginx

MySQL down or high connections

Too many requests on Java app

StatefulSet replica mismatch

Email notifications triggered successfully when alert conditions are met

Outcome

Prometheus successfully monitored all services

Alerts were delivered via email, achieving the main goal


---

🛠 Setup Instructions
1️⃣ Create Kubernetes Cluster
eksctl create cluster --name monitoring-cluster --region eu-west-2 --nodegroup-name ng-1 --nodes 2 --nodes-min 1 --nodes-max 3
---
2️⃣ Deploy MySQL with Helm
helm repo add bitnami https://charts.bitnami.com/bitnami
helm upgrade --install my-mysql bitnami/mysql -n monitoring -f 2-mysql-chart-values-lke.yaml
---
3️⃣ Deploy Java Application
kubectl apply -f java-app.yaml -n monitoring
kubectl apply -f java-app-ingress.yaml -n monitoring
---
4️⃣ Deploy Nginx Ingress Controller
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm upgrade --install nginx-ingress ingress-nginx/ingress-nginx -n monitoring -f nginx-ingress-chart-values.yaml
---
5️⃣ Deploy Prometheus Operator
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm upgrade --install monitoring-stack prometheus-community/kube-prometheus-stack -n monitoring
---
6️⃣ Configure Metrics Scraping & Alerts
kubectl apply -f 2-java-service-monitor.yaml -n monitoring
kubectl apply -f 2-mysql-chart-values-lke.yaml -n monitoring
kubectl apply -f 3-nginx-alert-rules.yaml -n monitoring
kubectl apply -f 3-mysql-alert-rules.yaml -n monitoring
kubectl apply -f 3-java-alert-rules.yaml -n monitoring
kubectl apply -f 4-email-secret.yaml -n monitoring
---
7️⃣ Access Prometheus & Grafana
kubectl port-forward svc/prometheus-monitoring-stack-kube-prom-prometheus 9090:9090 -n monitoring
kubectl port-forward svc/monitoring-stack-grafana 3000:80 -n monitoring
---

🧑 Personal Learnings

Learned Kubernetes StatefulSets, Services, and Ingress controllers

Worked with Helm charts and custom values for MySQL and Nginx

Configured Prometheus metrics scraping and alerting

Implemented email notifications for critical issues

Gained experience in debugging pending pods and PVC issues

Practiced CI/CD principles for monitoring infrastructure
---
🔗 References

Prometheus Helm Charts

Bitnami MySQL Helm Chart

Nginx Ingress Helm Chart
