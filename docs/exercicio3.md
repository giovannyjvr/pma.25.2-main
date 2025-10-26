
---

### 10. `docs/exercicio3.md`

```markdown
# Exercício 3 · Exchange API (Python)

!!! tip "Objetivo oficial"
    Criar um microsserviço de câmbio que converte moedas (`exchange-service`).  
    Esse serviço **tem que ser em Python** e deve consumir uma API externa de taxas de câmbio (por ex. AwesomeAPI, ExchangeRate-API etc.).  
    Ele expõe um endpoint REST tipo `GET /exchange/{from}/{to}` que devolve a taxa de conversão e metadados (data, id da conta, etc.).  
    (Baseado no enunciado "3. Exchange API")  
    Referência: https://insper.github.io/platform/exercises/exchange/

## O que eu implementei
- Serviço `exchange-service` escrito em Python (FastAPI).
- Endpoint principal:
  - `GET /exchange/{from}/{to}` → retorna quanto vale 1 unidade da moeda `{from}` em `{to}`.
- Esse serviço faz chamada HTTP para uma API externa de câmbio em tempo real.
- O gateway encaminha a chamada autenticada para o `exchange-service`.

```mermaid
flowchart LR
    gateway[API Gateway] --> exchange[exchange-service (Python)]
    exchange --> extAPI[(3rd-party FX API)]
