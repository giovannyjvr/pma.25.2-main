# Exercício 2 · Order API

!!! summary "Objetivo oficial"
    Desenvolver um microsserviço para representar pedidos de compra (ordens), conectando-se a outros serviços como o `product-service`.  
    A API deve permitir criar pedidos com múltiplos produtos, listar pedidos existentes e consultar um pedido por ID.  
    O serviço deve persistir os dados, estar protegido por autenticação e ser acessado via Gateway.

---

### Links dos repositórios

[Order](https://github.com/giovannyjvr/pma.25.2-order)  
[Order-Service](https://github.com/giovannyjvr/pma.25.2-order-service)

---

## O que foi implementado

O microsserviço `order-service` foi desenvolvido para gerenciar ordens de compra dentro do ecossistema de microsserviços da aplicação.

Ele cumpre todos os requisitos do exercício proposto:

- Permite:
  - **Criar um pedido**, com múltiplos produtos e suas respectivas quantidades;
  - **Listar todos os pedidos existentes**, com detalhes de produtos incluídos;
  - **Buscar um pedido específico por ID** e visualizar seu conteúdo completo.

- Cada pedido é composto por:
  - Identificador único (`id`)
  - Lista de produtos (`productId`, quantidade, valor)
  - Status do pedido (`OPEN`, `CONFIRMED`, etc.)
  - Timestamp de criação

- O serviço persiste os pedidos em banco de dados usando Spring Data JPA.

- A lógica de negócio consulta o `product-service` para obter dados reais de produto e calcular corretamente o valor total dos pedidos.

- A API está integrada ao **Gateway**, que protege o acesso com autenticação via JWT.

---

## Endpoints implementados

Todos os endpoints seguem o prefixo base `/orders`:

| Método HTTP | Rota               | Função                                         |
|-------------|--------------------|------------------------------------------------|
| `POST`      | `/orders`          | Cria um novo pedido com múltiplos produtos.    |
| `GET`       | `/orders`          | Lista todos os pedidos já registrados.         |
| `GET`       | `/orders/{id}`     | Retorna os detalhes completos de um pedido.    |

---

## Observações finais

Todos os requisitos propostos no exercício foram cumpridos:

- O serviço aceita múltiplos produtos por pedido e busca os dados reais desses produtos no `product-service`.
- Os pedidos são persistidos corretamente no banco de dados.
- A API segue boas práticas REST, com uso de `POST`, `GET` e status HTTP adequados.
- O acesso ao serviço está protegido via Gateway, com autenticação de usuários.

O `order-service` está pronto para integrar o fluxo de compra completo da aplicação, servindo como elo entre o cliente final e o catálogo de produtos.

