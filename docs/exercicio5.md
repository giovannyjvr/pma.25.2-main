# Exercício · Minikube / Kubernetes Local

!!! summary "Objetivo oficial"
    Utilizar o ambiente local com Minikube (ou cluster Kubernetes equivalente) para deployment de microsserviços.  
    O objetivo é demonstrar que os microsserviços desenvolvidos (product‑service, order‑service, auth‑service, gateway, etc) podem subir em Kubernetes local, com os manifests adequados (Deployment, Service, ConfigMap, Secret) e configurar corretamente as dependências entre serviços.

---

## ✅ O que foi implementado

Todos os microsserviços foram adaptados para rodar corretamente em um cluster Kubernetes local (via **Minikube**). Os principais pontos implementados foram:

- **Manifests YAML completos** (`Deployment` + `Service`) para cada microsserviço.
- Uso de imagens Docker customizadas publicadas no DockerHub (ex: `giovannyjvr/order:latest`).
- Configuração de variáveis de ambiente via `ConfigMap` e `Secret`, permitindo segurança e flexibilidade.
- Recursos limitados por container (`cpu` e `memória`) para controle de consumo.
- Comunicação entre microsserviços feita por nomes DNS internos (ex: `product-service`, `auth-service`, etc).
- Exposição do `gateway-service` como ponto de entrada externo via `NodePort`.

---

## 📦 Repositórios ativados

| Microsserviço | Repositório GitHub |
|---------------|---------------------|
| Auth          | [Auth-Service](https://github.com/giovannyjvr/pma.25.2-auth-service) |
| Account       | [Account-Service](https://github.com/giovannyjvr/pma.25.2-store.account-service) |
| Order         | [Order-Service](https://github.com/giovannyjvr/pma.25.2-order-service) |
| Product       | [Product-Service](https://github.com/giovannyjvr/pma.25.2-product-service) |
| Gateway       | [Gateway-Service](https://github.com/giovannyjvr/pma.25.2-gateway-service) |

---

## 📁 Estrutura de arquivos

Cada serviço possui um diretório `k8s/` contendo o manifest:

```
api/
└── account-service/
    └── k8s/
        └── k8s.yaml
```

---

## 📄 Exemplo de manifest (Order Service)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order
spec:
  replicas: 1
  selector:
    matchLabels:
      app: order
  template:
    metadata:
      labels:
        app: order
    spec:
      containers:
        - name: order
          image: giovannyjvr/order:latest
          imagePullPolicy: Always
          ports:
            - containerPort: 8080
          env:
            - name: POSTGRES_DB
              valueFrom:
                configMapKeyRef:
                  name: postgres-configmap
                  key: POSTGRES_DB
            - name: DATABASE_USERNAME
              valueFrom:
                secretKeyRef:
                  name: postgres-secrets
                  key: POSTGRES_USER
            - name: DATABASE_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: postgres-secrets
                  key: POSTGRES_PASSWORD
            - name: DATABASE_URL
              value: "jdbc:postgresql://postgres:5432/$(POSTGRES_DB)"
          resources:
            requests:
              memory: "200Mi"
              cpu: "50m"
            limits:
              memory: "300Mi"
              cpu: "200m"
---
apiVersion: v1
kind: Service
metadata:
  name: order
  labels:
    app: order
spec:
  type: ClusterIP
  ports:
    - port: 80
      protocol: TCP
      targetPort: 8080
  selector:
    app: order
```
