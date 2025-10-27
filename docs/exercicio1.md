# Exercício 1 · Product API

!!! summary "Objetivo oficial"
    Criar um microsserviço independente para gerenciar produtos de uma loja, utilizando arquitetura de microsserviços.  
    O serviço deve expor uma API REST com operações básicas de CRUD (sem atualização), persistir os dados e ser acessado via Gateway protegido por autenticação.

---
### link Dos repositórios

[Product](https://github.com/giovannyjvr/pma.25.2-product-) 

[Product-Service](https://github.com/giovannyjvr/pma.25.2-product-service)

---
## O que foi implementado

Este microsserviço (`product-service`) atende integralmente ao que foi solicitado no exercício proposto:

- A API permite:
  - **Cadastrar** produtos com nome, preço e unidade;
  - **Listar** todos os produtos cadastrados;
  - **Buscar** um produto específico por ID;
  - **Remover** um produto pelo ID.

- Os dados são **persistidos em banco de dados** por meio de uma entidade `Product`, com identificador do tipo UUID.

- O serviço foi implementado como um **microsserviço autônomo**, com endpoints REST expostos via `Spring Boot`, estruturado em camadas (controller, service, repository).

- A API está integrada ao **Gateway**, que protege as rotas com autenticação e valida tokens JWT antes de permitir o acesso.

---

## Endpoints implementados

Todos os endpoints seguem o prefixo base `/products`:

| Método HTTP | Rota                 | Função                                      |
|-------------|----------------------|---------------------------------------------|
| `POST`      | `/products`          | Cadastra um novo produto.                   |
| `GET`       | `/products`          | Lista todos os produtos existentes.         |
| `GET`       | `/products/{id}`     | Retorna um produto específico por ID.       |
| `DELETE`    | `/products/{id}`     | Remove um produto pelo ID informado.        |

---

## Observações finais

Todos os requisitos propostos no enunciado foram cumpridos:
- Os endpoints obrigatórios estão disponíveis.
- O serviço persiste os dados corretamente.
- A autenticação via gateway está funcionando como camada intermediária de proteção da API.

O `product-service` está pronto para ser consumido por outros microsserviços ou front-ends conectados à arquitetura.

