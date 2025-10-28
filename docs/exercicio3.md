# Exercício 3 · Exchange API

!!! summary "Objetivo oficial"
    Criar um microsserviço independente para realizar conversão entre moedas,  
    utilizando dados de uma API externa ou fonte local.  
    A API deve permitir consultar a taxa de câmbio entre duas moedas  
    e converter valores com base nessa taxa.  
    O acesso à API deve ser feito via Gateway com autenticação.

---

### Links dos repositórios

[Exchange](https://github.com/giovannyjvr/exchange-api)  

---

## O que foi implementado

Implementei o microsserviço `exchange-api`, responsável por realizar operações de câmbio (conversão de valores entre moedas) no ecossistema da aplicação.

Este serviço cumpre integralmente os requisitos propostos no exercício:

- Permite:
  - **Consultar a taxa de câmbio** entre duas moedas específicas;
  - **Converter** um valor de uma moeda para outra usando a taxa correspondente.

- A API expõe dois endpoints principais:
  - Um `GET` para retornar a taxa atual entre duas moedas;
  - Um `POST` para realizar a conversão de valores.

- O serviço pode usar:
  - **Uma API externa de câmbio**, como fonte de taxa (Ex: OpenExchange, AwesomeAPI, etc);
  - Ou uma **tabela local simulada** de taxas (em modo offline/teste).

- O acesso à API está protegido via **Gateway**, que realiza a autenticação por JWT antes de encaminhar as requisições.

---

## Endpoints implementados

Todos os endpoints seguem o prefixo base `/exchange`.

| Método HTTP | Rota                             | Função                                               |
|-------------|----------------------------------|------------------------------------------------------|
| `GET`       | `/exchange/rate?from=USD&to=BRL` | Retorna a taxa de câmbio da moeda `from` para `to`. |
| `POST`      | `/exchange/convert`              | Recebe um valor e converte de `from` para `to`.     |

---

### Exemplo de uso

#### `GET /exchange/rate`

```http
GET /exchange/rate?from=USD&to=EUR
Authorization: Bearer <token>
