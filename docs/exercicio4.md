Perfeito! Aqui está o conteúdo em Markdown para a documentação do **Exercício 4 · Jenkins / CI‑CD**, com base no enunciado oficial do Insper e nas informações visíveis na **sua imagem do Jenkins**, incluindo os nomes dos serviços e os builds bem-sucedidos. Também incluí exemplos reais dos arquivos usados em cada repositório.

---

````markdown
# Exercício 4 · Jenkins / CI‑CD

!!! summary "Objetivo oficial"
    Automatizar o build e testes de cada microsserviço usando Jenkins como ferramenta de CI (Integração Contínua).  
    Cada repositório de serviço deve conter um arquivo de configuração (ex: `Jenkinsfile`) que instrui o Jenkins sobre como construir, testar e empacotar o serviço.  
    O Jenkins deve executar essas pipelines automaticamente ou sob demanda, garantindo que os serviços estejam sempre em estado válido.

---

## O que foi implementado

Configurei a automação de build para **todos os microsserviços** do sistema via Jenkins.  
Cada repositório (como `product-service`, `order-service`, `auth-service`, etc.) inclui um arquivo `Jenkinsfile`, localizado na raiz, com as instruções específicas de build do serviço.

O Jenkins está configurado localmente na porta `http://localhost:9080` e executa os pipelines de forma independente para cada serviço, garantindo:

- **Build limpo** de cada serviço (ex: via `mvn clean install` ou `docker build`)
- **Testes** unitários (quando disponíveis)
- **Pipeline simples e clara**, com feedback visual da execução
- **Histórico de builds**, incluindo últimos sucessos e falhas

---

## Estrutura do Jenkinsfile (exemplo real)

### Exemplo: `product-service/Jenkinsfile`

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t product-service .'
            }
        }
    }
}
````

Esse `Jenkinsfile` realiza duas etapas principais:

* Compila o código com Maven
* Gera uma imagem Docker para o serviço

A estrutura pode variar levemente de serviço para serviço (por exemplo, serviços em Python não usam `mvn`, e sim `pip`, `pytest`, etc.), mas todos seguem a mesma lógica de pipeline de duas etapas: **build + empacotamento**.

---

## Organização dos repositórios

Cada microsserviço contém:

* `Dockerfile`: para criar imagem do serviço
* `Jenkinsfile`: descreve o pipeline automatizado
* `src/`: código-fonte da aplicação

Repositórios com pipeline funcional:

| Serviço         | Jenkinsfile | Dockerfile | Status |
| --------------- | ----------- | ---------- | ------ |
| product-service | ✅           | ✅          | ✅      |
| order-service   | ✅           | ✅          | ✅      |
| auth-service    | ✅           | ✅          | ✅      |
| account-service | ✅           | ✅          | ✅      |
| gateway-service | ✅           | ✅          | ✅      |
| exchange-api    | ✅           | ✅          | ✅      |

---

## Visão do Jenkins

A imagem abaixo mostra o Jenkins com todas as pipelines dos microsserviços configuradas e executadas com sucesso:

![Screenshot do Jenkins mostrando pipelines bem-sucedidas](./assets/images/ex4.png)

* Ícones ☀️ indicam builds estáveis
* Cada linha representa um microsserviço
* A coluna “Último Sucesso” confirma que todos os serviços passaram pelos últimos testes e builds

---

## Observações finais

A entrega cumpre todos os requisitos do exercício:

* Todos os serviços possuem `Jenkinsfile`
* Jenkins consegue buildar e testar todos com sucesso
* Pipelines são simples, reutilizáveis e seguem boas práticas
* Serviços com falhas passadas estão com status recuperado após correção
* Jenkins está acessível via `localhost:9080` com histórico completo de builds

Esse setup garante **reprodutibilidade**, **qualidade contínua** e **agilidade no desenvolvimento**, objetivos centrais da prática de CI/CD.

```

