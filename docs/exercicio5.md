
# Exercício · Minikube / Kubernetes Local

!!! summary "Objetivo oficial"
    Utilizar o ambiente local com Minikube (ou cluster Kubernetes equivalente) para deployment de microsserviços.  
    O objetivo é demonstrar que os microsserviços desenvolvidos (product‑service, order‑service, auth‑service, gateway, etc) podem subir em Kubernetes local, com os manifests adequados (Deployment, Service, ConfigMap, Secret) e configurar corretamente as dependências entre serviços.

---

## O que foi implementado

Para este exercício, todos os microsserviços foram adaptados para rodar em um cluster Kubernetes local ou equivalente. Em particular:

- Foram criados **manifests YAML** de `Deployment` e `Service` para cada microsserviço (ex: `order‑service`, `product‑service`, etc).  
- Cada serviço segue a boa prática de containerização, apontando para sua imagem Docker (ex: `giovannyjvr/order:latest`).  
- As aplicações foram configuradas para buscar suas variáveis de ambiente via `ConfigMap` e `Secret` em Kubernetes (por ex: `POSTGRES_DB`, `DATABASE_URL`, `DATABASE_USERNAME`, `DATABASE_PASSWORD`).  
- Configuração de recursos (`requests` e `limits`) foi aplicada para cada `Deployment`, garantindo uso responsável de CPU/memória.  
- Os microsserviços foram implantados em Minikube ou cluster local para validar que a infraestrutura de microsserviços funciona em orquestrador real.  
- Os serviços se comunicam entre si internamente no cluster, usando os nomes DNS dos Services (ex: `order-service` → `product-service`) e o `gateway-service` atua como interface de entrada externa.

---
repositórios ativados: 
| Microservice  |Implementação                                                              |
|---------------------|-----------------------------------------------------------------|
| Auth    | [Auth-Service](https://github.com/giovannyjvr/pma.25.2-auth-service)|
| Account | [Account-Service](https://github.com/giovannyjvr/pma.25.2-store.account-service)|
| Order | [Order-Service](https://github.com/giovannyjvr/pma.25.2-order-service)|
| Product | [Product-Service](https://github.com/giovannyjvr/pma.25.2-product-service)|
| Gateway | [Gateway](https://github.com/giovannyjvr/pma.25.2-gateway-service)|

Exemplo de estrutura feita:
```text
api/
└── account-service/
    └── k8s/
        └── k8s.yaml
```

## Exemplo de manifest YAML (Order Service)

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
