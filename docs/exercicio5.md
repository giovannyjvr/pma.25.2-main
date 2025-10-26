
---

### 12. `docs/exercicio5.md`

```markdown
# Exercício 5 · MiniKube / Kubernetes Local

!!! warning "Objetivo oficial"
    Subir TODOS os microsserviços no MESMO cluster Kubernetes (local ou cloud).  
    Você deve criar manifestos `k8s.yaml` com pelo menos:  
    - Secrets  
    - ConfigMap  
    - Deployment  
    - Service  
    E provar (com vídeo / prints) que todos estão rodando no mesmo cluster (pode ser Minikube, Kind ou Kubernetes do Docker Desktop).  
    (Baseado no enunciado "5. MiniKube")  
    Referência: https://insper.github.io/platform/exercises/minikube/

## O que eu implementei
- Ativei Kubernetes local (ex: Minikube).
- Para cada serviço (`account-service`, `auth-service`, `gateway-service`, `product-service`, `order-service`) criei um diretório `k8s/` com um `k8s.yaml`.
- Os manifests definem:
  - Secrets (credenciais sensíveis, tokens, etc.)
  - ConfigMap (config não secreta)
  - Deployment (pods, imagem Docker que veio do Jenkins)
  - Service (tipo ClusterIP pro tráfego interno)

Exemplo de estrutura esperada:
```text
api/
└── account-service/
    └── k8s/
        └── k8s.yaml
