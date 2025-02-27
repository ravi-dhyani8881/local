Kubernetes Deployment Model for Multi-Tenant Graph Services

📌 Overview

This document describes a multi-tenant Kubernetes deployment model that ensures:

✅ Each organization has isolated resources (Org A & Org B cannot share resources).✅ Sub-orgs can deploy graphs independently but share resources within the same org.✅ Each graph has 3 microservices:

Spring Boot REST API

Spring Boot Graph API

DB Layer✅ Unique URL for each graph in Kubernetes.

🏗 Deployment Model Overview

Multi-Tenancy Structure in Kubernetes

k8s-cluster
├── org-a-namespace
│   ├── dev
│   │   ├── graph-1
│   │   │   ├── spring-boot-rest
│   │   │   ├── spring-boot-graph
│   │   │   ├── db-layer
│   │   ├── graph-2
│   │   │   ├── spring-boot-rest
│   │   │   ├── spring-boot-graph
│   │   │   ├── db-layer
│   ├── prod
│   │   ├── graph-1
│   │   │   ├── spring-boot-rest
│   │   │   ├── spring-boot-graph
│   │   │   ├── db-layer
├── org-b-namespace
│   ├── stg
│   │   ├── graph-1
│   │   │   ├── spring-boot-rest
│   │   │   ├── spring-boot-graph
│   │   │   ├── db-layer
│   ├── qa
│   │   ├── graph-2
│   │   │   ├── spring-boot-rest
│   │   │   ├── spring-boot-graph
│   │   │   ├── db-layer

🚀 Deployment Strategy

1️⃣ Multi-Tenant Isolation Using Namespaces

Each Org gets a dedicated Namespace:

kubectl create namespace org-a
kubectl create namespace org-b

Ensures no resource sharing between organizations.

2️⃣ Deploying a Graph in Kubernetes

Each graph is deployed within an org’s namespace under the respective environment.

📌 Example: Deploy Graph-1 in Org-A (Dev Environment)

helm install graph-1 ./graph-helm-chart --namespace org-a --set environment=dev

📌 Example: Deploy Graph-2 in Org-B (QA Environment)

helm install graph-2 ./graph-helm-chart --namespace org-b --set environment=qa

📜 Kubernetes Deployment YAML (For One Graph)

Each graph consists of 3 services: REST API, Graph API, and DB.

1️⃣ Spring Boot REST API Deployment

apiVersion: apps/v1
kind: Deployment
metadata:
  name: graph-rest
  namespace: {{ .Values.namespace }}
spec:
  replicas: 2
  selector:
    matchLabels:
      app: graph-rest
  template:
    metadata:
      labels:
        app: graph-rest
    spec:
      containers:
      - name: graph-rest
        image: {{ .Values.restApi.image }}
        ports:
        - containerPort: 8080

2️⃣ Spring Boot Graph API Deployment

apiVersion: apps/v1
kind: Deployment
metadata:
  name: graph-api
  namespace: {{ .Values.namespace }}
spec:
  replicas: 2
  selector:
    matchLabels:
      app: graph-api
  template:
    metadata:
      labels:
        app: graph-api
    spec:
      containers:
      - name: graph-api
        image: {{ .Values.graphApi.image }}
        ports:
        - containerPort: 9090

3️⃣ Database Deployment

apiVersion: apps/v1
kind: Deployment
metadata:
  name: graph-db
  namespace: {{ .Values.namespace }}
spec:
  replicas: 1
  selector:
    matchLabels:
      app: graph-db
  template:
    metadata:
      labels:
        app: graph-db
    spec:
      containers:
      - name: graph-db
        image: {{ .Values.db.image }}
        ports:
        - containerPort: 5432

🌍 Unique URL Mapping with Ingress

Each graph gets a unique URL based on the org and environment.

🔹 Ingress Example (for Org-A, Dev, Graph-1)

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: graph-ingress
  namespace: org-a
spec:
  rules:
  - host: graph1.dev.org-a.company.com
    http:
      paths:
      - path: /rest
        pathType: Prefix
        backend:
          service:
            name: graph-rest
            port:
              number: 8080
      - path: /graph
        pathType: Prefix
        backend:
          service:
            name: graph-api
            port:
              number: 9090

🚀 Now you can access:

Spring REST API → http://graph1.dev.org-a.company.com/rest

Graph API → http://graph1.dev.org-a.company.com/graph

📦 Using Helm for Multi-Graph Deployment

Since we deploy multiple graphs dynamically, we use Helm.

🔹 values.yaml (Helm Config File)

namespace: org-a
environment: dev

restApi:
  image: myrepo/graph-rest:latest

graphApi:
  image: myrepo/graph-api:latest

db:
  image: myrepo/graph-db:latest

🔹 Deploying with Helm

helm install graph-1 ./graph-helm-chart --namespace org-a --set environment=dev

🎯 Key Takeaways

Feature

Implementation

Multi-Tenancy (Org Isolation)

Namespaces per Org (org-a, org-b)

Sub-Orgs & Environments

Deploy graphs under the same environment (dev, stg, prod)

Graphs as Microservices

Each graph has 3 services: REST API, Graph API, DB

Unique URLs per Graph

Ingress rules per graph (graph1.dev.org-a.company.com)

Dynamic Deployment

Helm-based deployments for flexibility

🚀 Now you have a fully multi-tenant Kubernetes model with isolated deployments, dynamic scaling, and unique URL access! 🎯🔥

