
---

### 11. `docs/exercicio4.md`

```markdown
# Exercício 4 · Jenkins / CI-CD

!!! note "Objetivo oficial"
    Montar um pipeline Jenkins que faz build e deploy automático dos microsserviços.  
    Pipeline padrão:
    1. SCM (pegar código do repo)
    2. Dependencies
    3. Build
    4. Push da imagem Docker (multi-arch) pro Docker Hub
    5. Deploy em Kubernetes  
    Todos os microsserviços (account-service, auth-service, gateway-service, product-service, order-service) devem ir para o mesmo cluster.  
    (Baseado no enunciado "4. Jenkins")  
    Referência: https://insper.github.io/platform/exercises/jenkins/

## O que eu implementei
- Jenkinsfile com stages: Dependecies → Build → Build & Push Image → Deploy.
- Uso de `withCredentials` pra logar no Docker Hub dentro do pipeline.
- Build multi-plataforma (`linux/arm64`, `linux/amd64`) usando `docker buildx`.
- Tag automática `latest` e tag usando o `BUILD_ID`.

Exemplo (baseado no modelo do professor adaptado pra mim):

```groovy
pipeline {
    agent any
    environment {
        SERVICE = 'account'
        NAME = "SEU_DOCKERHUB/${env.SERVICE}"
    }
    stages {
        stage('Dependecies') {
            steps {
                build job: 'account', wait: true
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Build & Push Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credential',
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'TOKEN')]) {
                    sh "docker login -u $USERNAME -p $TOKEN"
                    sh "docker buildx create --use --platform=linux/arm64,linux/amd64 --node multi-platform-builder-${env.SERVICE} --name multi-platform-builder-${env.SERVICE}"
                    sh "docker buildx build --platform=linux/arm64,linux/amd64 --push --tag ${env.NAME}:latest --tag ${env.NAME}:${env.BUILD_ID} -f Dockerfile ."
                    sh "docker buildx rm --force multi-platform-builder-${env.SERVICE}"
                }
            }
        }
        // opcional: stage('Deploy to K8s') { ... }
    }
}
