# Portfólio · Plataformas, Microsserviços e APIs

!!! info "Resumo"
    Este site documenta minha implementação dos exercícios da disciplina **Plataformas, Microsserviços, DevOps e APIs** (Insper).  
    A ideia é mostrar o que eu construí, como implementei cada serviço e evidências (prints / logs / comandos).

## Sobre mim 👩‍💻
- Nome: Giovanny Russo 
- Stack principal: Spring Boot, Python (FastAPI), Docker, Jenkins, Kubernetes.
### Arquitetura de Microsserviços

| Microservice  | Interface (API / Endpoint Público)                              | Implementação                                                              |
|---------------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| Auth    | [Auth](https://github.com/giovannyjvr/pma.25.2-auth) | [Auth-Service](https://github.com/giovannyjvr/pma.25.2-auth-service)|
| Account | [Account](https://github.com/giovannyjvr/pma.25.2-store.account) |[Account-Service](https://github.com/giovannyjvr/pma.25.2-store.account-service)|
| Order | [Order](https://github.com/giovannyjvr/pma.25.2-order)| [Order-Service](https://github.com/giovannyjvr/pma.25.2-order-service)|
| Product | [Product](https://github.com/giovannyjvr/pma.25.2-product-)| [Product-Service](https://github.com/giovannyjvr/pma.25.2-product-service)|
| Gateway |         | [Gateway](https://github.com/giovannyjvr/pma.25.2-gateway-service)|
| Exchange |           | [Exchange](https://github.com/giovannyjvr/exchange-api)|

## Visão Geral da Arquitetura

A disciplina trabalha com uma arquitetura baseada em microsserviços (account-service, auth-service, product-service, order-service, exchange-service), um API Gateway e um banco de dados.  
Cada microsserviço é isolado, mas todos se conversam via HTTP dentro de uma camada "confiável" (Trusted Layer).

```mermaid
flowchart LR
    subgraph Internet
        client[Cliente / Front-end]
    end

    client --> gateway[API Gateway]

    subgraph "Trusted Layer"
        gateway --> account[account-service]
        gateway --> auth[auth-service]
        gateway --> product[product-service]
        gateway --> order[order-service]
        gateway --> exchange[exchange-service (Python)]
        account --> db[(Database)]
        product --> db
        order --> db
        order --> product
        exchange --> extAPI[(3rd-party FX API)]
    end
