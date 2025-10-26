# Portfólio · Plataformas, Microsserviços e APIs

!!! info "Resumo"
    Este site documenta minha implementação dos exercícios da disciplina **Plataformas, Microsserviços, DevOps e APIs** (Insper).  
    A ideia é mostrar o que eu construí, como implementei cada serviço e evidências (prints / logs / comandos).

## Sobre mim 👩‍💻
- Nome: SEU NOME AQUI  
- Semestre / Grupo: COLOCAR AQUI  
- Repositórios envolvidos: COLOCAR LINKS AQUI  
- Stack principal: Spring Boot, Python (FastAPI), Docker, Jenkins, Kubernetes.

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
