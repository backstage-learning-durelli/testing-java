# testing-java

Projeto Spring Boot criado com Backstage, incluindo CI/CD e deploy automático via ArgoCD.

[![CI/CD](https://github.com/backstage-learning-durelli/testing-java/actions/workflows/ci-cd.yaml/badge.svg)](https://github.com/backstage-learning-durelli/testing-java/actions/workflows/ci-cd.yaml)

## 🚀 Tecnologias

- Java 17
- Spring Boot 3.2.0
- Spring Web
- Spring Actuator
- Maven

## 📋 Pré-requisitos

- Java 17 ou superior
- Maven 3.6 ou superior

## 🔧 Como rodar

### Desenvolvimento

```bash
# Compilar o projeto
mvn clean install

# Executar a aplicação
mvn spring-boot:run
```

A aplicação estará disponível em `http://localhost:8080`

## 🌐 Endpoints

### Hello Endpoint
```
GET http://localhost:8080/api/hello
```

Resposta:
```json
{
  "message": "Hello from testing-java!"
}
```

### Health Check
```
GET http://localhost:8080/actuator/health
```

## 🧪 Testes

```bash
# Executar testes
mvn test
```

## 🐳 Docker

```bash
# Build da imagem
docker build -t testing-java:latest .

# Executar container
docker run -p 8080:8080 testing-java:latest
```

## 🔄 CI/CD e Deploy

### Pipeline Automático (GitHub Actions)

Este projeto possui um pipeline de CI/CD configurado que é executado automaticamente:

**Em Pull Requests:**
- Executa testes unitários

**No branch `main`:**
1. ✅ Executa testes
2. ✅ Builda a aplicação com Maven
3. ✅ Cria imagem Docker
4. ✅ Publica no Docker Hub (`durellirsd/testing-java`)
5. ✅ Atualiza manifestos Kubernetes com nova tag
6. ✅ Commit automático da mudança

### Deploy via ArgoCD

O deploy é gerenciado pelo ArgoCD usando GitOps:

- **Application**: `testing-java`
- **Namespace**: `testing-java`
- **Sync Policy**: Automático (prune + self-heal)

**Acessar no ArgoCD:**
```bash
# Via CLI
argocd app get testing-java
argocd app sync testing-java

# Ver logs
kubectl logs -n testing-java -l app=testing-java -f
```

### Estrutura de Deploy

```
manifests/
├── k8s/
│   ├── deployment.yaml  # Deployment com 2 réplicas
│   ├── service.yaml     # ClusterIP na porta 80
│   └── ingress.yaml     # Ingress (opcional)
```

**Verificar recursos no Kubernetes:**
```bash
# Ver todos os recursos
kubectl get all -n testing-java

# Acessar a aplicação (port-forward)
kubectl port-forward -n testing-java svc/testing-java 8080:80
curl http://localhost:8080/api/hello

