# Exercício 1 · Product API

!!! summary "Objetivo oficial"
    Criar uma API REST para a loja, expondo recursos de `product`.  
    Essa API precisa permitir cadastrar produtos, listar produtos, buscar por ID e remover.  
    O serviço deve ser independente (um microsserviço próprio), persistir dados no banco e ser acessado via gateway com autenticação.  
    Baseado no enunciado "1. Product API" (`Product API` do curso).

---
### link Dos repositórios

[Product](https://github.com/giovannyjvr/pma.25.2-product-) 

[Product-Service](https://github.com/giovannyjvr/pma.25.2-product-service)

--- 

## Visão geral do serviço

O `product-service` é o microsserviço responsável por gerenciar produtos do catálogo.  
Ele expõe rotas HTTP/JSON para criar, consultar e apagar produtos.

Cada **Product** tem basicamente:
- `id` (UUID gerado pelo backend)
- `name` (nome do produto)
- `price` (preço numérico)
- `unit` (unidade de venda, ex: `"un"`, `"kg"`, `"box"`)

Esse serviço é pensado para ser consumido por outros serviços como `order-service` (que precisa saber quais produtos existem e quanto custam) e pelo `gateway` (que expõe isso pro mundo externo com autenticação/autorização).

---

## Como está implementado (tecnologia e arquitetura interna)

**Stack técnica:**
- **Linguagem:** Java + Spring Boot.
- **Camadas padrão de API REST Spring:**
  - `ProductResource` → Controller REST (endereços HTTP).
  - `ProductService` → Regras de negócio e validação.
  - `ProductRepository` → Persistência usando Spring Data JPA.
  - `ProductModel` → Entidade JPA anotada com `@Entity` / `@Table("product")`.
  - `Product` → Modelo de domínio puro (objeto simples com `id`, `name`, `price`, `unit`).
  - `ProductIn` / `ProductOut` → DTOs de entrada e saída usados na API.
  - `ProductParser` → conversão entre DTO ↔ domínio ↔ entidade.

**Banco de dados / persistência:**
- A entidade `ProductModel` é marcada com `@Entity` e mapeada na tabela `product`.
- Campos persistidos:
  - `id` (`@Id`, `@GeneratedValue(strategy = GenerationType.UUID)`)
  - `name`
  - `price`
  - `unit`
- O `ProductRepository` estende `CrudRepository<ProductModel, String>`, então a API pode:
  - salvar (`save`)
  - buscar por ID (`findById`)
  - listar todos (`findAll`)
  - apagar (`deleteById`)
  - e também fazer busca customizada `findByNameContainingIgnoreCase(name)` para pesquisa parcial pelo nome.

**Validação e status HTTP:**
- Antes de criar um produto novo, o `ProductService` faz validações manuais:
  - `name` não pode ser vazio
  - `price` precisa existir e ser maior que zero
  - `unit` não pode ser vazia
- Se algo estiver inválido, responde `400 Bad Request` via `ResponseStatusException(HttpStatus.BAD_REQUEST, "...")`.
- Para recursos não encontrados, responde `404 Not Found`.

**Integração com o resto do sistema:**
- Em produção, esse serviço normalmente roda em um container Docker próprio.
- Ele é acessado pelo API Gateway, que aplica autenticação (JWT) e roteia `/products/...` para esse serviço dentro da Trusted Layer.
- O `order-service` consulta o `product-service` para montar pedidos com dados reais de produto/preço, em vez de duplicar essas informações.

---

## Endpoints expostos

Todos os endpoints abaixo estão dentro do prefixo base:
**`/products`**

| Método HTTP          | Rota                             | O que faz                                                                 | Resposta / Status                                        |
|----------------------|----------------------------------|---------------------------------------------------------------------------|----------------------------------------------------------|
| `POST`               | `/products`                      | Cria um novo produto a partir de um JSON enviado no body (`ProductIn`).  | `201 Created` + `Location: /products/{id}` e corpo `ProductOut`. |
| `GET`                | `/products`                      | Lista todos os produtos cadastrados.                                     | `200 OK` com lista de `ProductOut`.                      |
| `GET`                | `/products/{id}`                 | Retorna um produto específico pelo `id`.                                 | `200 OK` com `ProductOut`, ou `404 Not Found`.           |
| `DELETE`             | `/products/{id}`                 | Remove um produto específico.                                            | `204 No Content`.                                        |
| `GET`                | `/products/search?name=TECLADO` | Busca produtos cujo nome contém o termo (case-insensitive).              | `200 OK` com lista filtrada.                             |

### Estrutura esperada de entrada (`POST /products`)
Exemplo de body (`ProductIn`):
```json
{
  "name": "Teclado Mecânico RGB",
  "price": 349.90,
  "unit": "un"
}
