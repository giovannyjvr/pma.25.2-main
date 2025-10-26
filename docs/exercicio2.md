
---

### 9. `docs/exercicio2.md`

```markdown
# Exercício 2 · Order API

!!! summary "Objetivo oficial"
    Criar a API de pedidos (`order-service`).  
    O usuário autenticado consegue gerar um pedido com itens que referenciam produtos existentes, e o serviço calcula total e data.  
    Também precisa listar os pedidos do próprio usuário e consultar o detalhe de um pedido específico, garantindo que ele só veja o que é dele.  
    (Baseado no enunciado "2. Order API")  
    Referência: https://insper.github.io/platform/exercises/order/

## O que eu implementei
- Criei o microsserviço `order-service`.
- Endpoints principais:
  - `POST /order` — cria um novo pedido para o usuário logado (lista de itens `{idProduct, quantity}`).
  - `GET /order` — lista todos os pedidos do usuário atual.
  - `GET /order/{id}` — detalhes de um pedido específico (se não for do usuário → `404`).
- Cálculo automático de total (somando preço * quantidade).
- Integração entre `order-service` e `product-service` (pra validar produto e preço).

```mermaid
flowchart LR
    gateway[API Gateway] --> order[order-service]
    order --> product[product-service]
    product --> db[(Database)]
    order --> db
