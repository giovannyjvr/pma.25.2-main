
---

### 8. `docs/exercicio1.md`

```markdown
# Exercício 1 · Product API

!!! summary "Objetivo oficial"
    Criar uma API REST para uma loja, com recursos `product` e `order`.  
    A API deve permitir cadastrar produtos, listar produtos, buscar por ID e apagar.  
    Para consumir a API, o usuário precisa estar autenticado.  
    (Baseado no enunciado "1. Product API")  
    Referência: https://insper.github.io/platform/exercises/product/

## O que eu implementei
- Criei o microsserviço `product-service`.
- Endpoints principais (exemplo):
  - `POST /product` — cria um produto novo.
  - `GET /product` — lista todos os produtos.
  - `GET /product/{id}` — retorna um produto específico.
  - `DELETE /product/{id}` — remove um produto específico.
- Integração com banco de dados para persistir os produtos.
- Integração com autenticação (gateway + auth-service).
- Status HTTP corretos (`201 created`, `200 ok`, `204 no content`, etc).

```mermaid
flowchart LR
    gateway[API Gateway] --> product[product-service]
    product --> db[(Database)]
    product --> order[order-service]
