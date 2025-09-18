# KUBERNETES APPLICATION MONITORING WITH PROMETHEUS***

---


##  **Explore My Work - *Click Each Badge*  to See Real Code & Configurations** 



 ***TECH STACK***

 
 
# Core Infrastructure & Orchestration:

[![AWS EKS](https://img.shields.io/badge/AWS_EKS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://github.com/CreatorVee/Prometheus-project-/blob/feature/solutions/1-configure-k8s.yaml)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://github.com/CreatorVee/Prometheus-project-/tree/feature/solutions/kubernetes-manifests)

---

# Monitoring Stack:

[![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)](https://github.com/CreatorVee/Prometheus-project-/blob/feature/solutions/kubernetes-manifests/2-java-service-monitor.yaml)
[![Grafana](https://img.shields.io/badge/Grafana-F46800?style=for-the-badge&logo=grafana&logoColor=white)](https://github.com/CreatorVee/Prometheus-project-/blob/feature/solutions/kubernetes-manifests/3-java-alert-rules.yaml)
[![AlertManager](https://img.shields.io/badge/AlertManager-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)](https://github.com/CreatorVee/Prometheus-project-/blob/feature/solutions/kubernetes-manifests/4-alert-manager-configuration.yaml)

---

# Application & Services:

[![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)](https://github.com/CreatorVee/Prometheus-project-/blob/feature/solutions/kubernetes-manifests/java-app.yaml)
[![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](https://github.com/CreatorVee/Prometheus-project-/blob/feature/solutions/kubernetes-manifests/1-mysql-chart-values-lke.yaml)
[![NGINX](https://img.shields.io/badge/NGINX-009639?style=for-the-badge&logo=nginx&logoColor=white)](https://github.com/CreatorVee/Prometheus-project-/blob/feature/solutions/kubernetes-manifests/java-app-ingress.yaml)
---

# Deployment & Configuration:

[![Helm](https://img.shields.io/badge/Helm-0F1689?style=for-the-badge&logo=helm&logoColor=white)](https://github.com/CreatorVee/Prometheus-project-/blob/feature/solutions/kubernetes-manifests/nginx-ingress-chart-values.yaml)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://github.com/CreatorVee/Prometheus-project-/blob/feature/solutions/kubernetes-manifests/java-app.yaml)
[![YAML](https://img.shields.io/badge/YAML-CB171E?style=for-the-badge&logo=yaml&logoColor=white)](https://github.com/CreatorVee/Prometheus-project-/tree/feature/solutions/kubernetes-manifests)


---


# ✅ PROJECT OVERVIEW***

***The goal of this project was to monitor a Java application, MySQL database, and Nginx Ingress controller deployed on a Kubernetes cluster, and to configure Prometheus to send email alerts when critical conditions occur.***

 🎯 Key objectives:

- Deploy Java app with MySQL backend in Kubernetes

- Expose the app via Nginx Ingress

- Monitor applications with Prometheus and Node/Service exporters

- Configure alert rules for critical scenarios

- Verify that alerts are delivered via email***

---

  ❌ ***PROBLEM***

Before this setup:

- Application health and performance metrics were not centrally monitored.

- Critical issues (e.g., MySQL down, too many connections, high HTTP errors) could go unnoticed.

- Manual checks were required, which are time-consuming and error-prone.

---

✅ ***SOLUTION***

- This project uses Kubernetes + Prometheus + Helm to create a self-monitoring environment:

- Java App & MySQL Deployment

- Java app deployed with 3 replicas

- MySQL deployed as a StatefulSet with 2 replicas

- Application communicates with MySQL database

- Ingress Controller

- Nginx deployed to route traffic to the Java application

- Monitoring & Alerts

- Prometheus Operator deployed using Helm

- Metrics scraping configured for Java app, MySQL, and Nginx

- Alerts configured for:

- HTTP 4xx errors on Nginx

- MySQL down or high connections

- Too many requests on Java app

- StatefulSet replica mismatch

- Email notifications triggered successfully when alert conditions are met

✅***OUTCOME***

-  ***Prometheus successfully monitored all services***

- ***Alerts were delivered via email, achieving the main goal***


---

📝 ***SETUP INSTRUCTIONS***

# Create Kubernetes Cluster
eksctl create cluster --name monitoring-cluster --region eu-west-2 --nodegroup-name ng-1 --nodes 2 --nodes-min 1 --nodes-max 3

---

# Deploy MySQL with Helm
helm repo add bitnami https://charts.bitnami.com/bitnami
helm upgrade --install my-mysql bitnami/mysql -n monitoring -f 2-mysql-chart-values-lke.yaml


---

# Deploy Java Application
kubectl apply -f java-app.yaml -n monitoring
kubectl apply -f java-app-ingress.yaml -n monitoring

---

# Deploy Nginx Ingress Controller
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm upgrade --install nginx-ingress ingress-nginx/ingress-nginx -n monitoring -f nginx-ingress-chart-values.yaml

---

# Deploy Prometheus Operator
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm upgrade --install monitoring-stack prometheus-community/kube-prometheus-stack -n monitoring

---

# Configure Metrics Scraping & Alerts
kubectl apply -f 2-java-service-monitor.yaml -n monitoring
kubectl apply -f 2-mysql-chart-values-lke.yaml -n monitoring
kubectl apply -f 3-nginx-alert-rules.yaml -n monitoring
kubectl apply -f 3-mysql-alert-rules.yaml -n monitoring
kubectl apply -f 3-java-alert-rules.yaml -n monitoring
kubectl apply -f 4-email-secret.yaml -n monitoring

---

# Access Prometheus & Grafana

kubectl port-forward svc/prometheus-monitoring-stack-kube-prom-prometheus 9090:9090 -n monitoring
kubectl port-forward svc/monitoring-stack-grafana 3000:80 -n monitoring



---

# 📚 ***PERSONAL LEARNINGS***

- Learned Kubernetes StatefulSets, Services, and Ingress controllers

- Worked with Helm charts and custom values for MySQL and Nginx

- Configured Prometheus metrics scraping and alerting

- Implemented email notifications for critical issues

- Gained experience in debugging pending pods and PVC issues

- Practiced CI/CD principles for monitoring infrastructure

---
# 📸 ***PHOTOGRAPHIC EVDIENCE***



<img width="1086" height="752" alt="2025-09-09_21h59_59" src="https://github.com/user-attachments/assets/724d3c8f-99bc-4672-93a3-af956b0daba8" />



---
🔗 ***REFERENCES***

Prometheus Helm Charts


