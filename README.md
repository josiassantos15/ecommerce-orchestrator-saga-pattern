# Saga Orchestration System

![Java](https://img.shields.io/badge/Java-17-orange)
![Angular](https://img.shields.io/badge/Angular-20-red)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.1-green)
![Kafka](https://img.shields.io/badge/Apache%20Kafka-3.5-blue)
![Docker](https://img.shields.io/badge/Docker-24.0-blue)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

## Sistema distribuído com arquitetura de microserviços implementando o padrão Saga Orquestrado para gerenciamento de transações distribuídas usando Java 17, Angular 20 e Apache Kafka.

### 🎯 Visão Geral
Este projeto implementa uma solução completa de Saga Orquestrado para garantir consistência eventual em operações distribuídas entre múltiplos microserviços. O sistema utiliza um orquestrador central para coordenar as transações e compensações entre os serviços.

### ✨ Funcionalidades
- **Orquestração de Sagas**: Gerenciamento centralizado de transações distribuídas
- **Compensação Automática**: Rollback em caso de falhas em qualquer etapa
- **Dashboard em Tempo Real**: Interface Angular para monitoramento de sagas
- **Resiliência a Falhas**: Sistema tolerante a falhas com retry policies
- **Event-Driven Architecture**: Comunicação assíncrona entre serviços via Kafka
- **Multi-banco de Dados**: PostgreSQL para dados transacionais e MongoDB para eventos
- **Containerização**: Todos os serviços executáveis via Docker
### 🏗️ Arquitetura
```text
┌─────────────────────────────────────────────────────────────┐
│                     Angular Dashboard (4200)                │
└─────────────────────────────────────────────────────────────┘
│
┌─────────────────────────────────────────────────────────────┐
│                     API Gateway (8080)                      │
└─────────────────────────────────────────────────────────────┘
│
┌─────────────────────────────────────────────────────────────┐
│                  Saga Orchestrator (8081)                   │
└─────────────────────────────────────────────────────────────┘
│
┌───────────┴───────────┐
│                       │
┌───────▼───────┐       ┌───────▼───────┐
│  Kafka Cluster│       │   PostgreSQL  │
│   (9092)      │       │   (5432)      │
└───────┬───────┘       └───────┬───────┘
│                       │
┌───────▼───────────────────────▼───────┐
│      Microservices (8082-8085)        │
└───────────────────────────────────────┘
│
┌───────────┴───────────┐
│                       │
┌───────▼───────┐       ┌───────▼───────┐
│   MongoDB     │       │   PostgreSQL  │
│   (27017)     │       │   (5433-5436) │
└───────────────┘       └───────────────┘
```

### 🛠️ Tecnologias
#### Backend (Microservices)
- **Java 17** com Spring Boot 3.1+
- **Spring Cloud** para configuração distribuída
- **Apache Kafka** 3.5 para mensageria
- **Spring Data JPA** + PostgreSQL (transações)
- **Spring Data MongoDB** para persistência de eventos
- **Resilience4j** para circuit breaker e retry
- **Spring Cloud Sleuth** + Zipkin para tracing
- **SpringDoc OpenAPI** para documentação

#### Frontend
- **Angular 20** com TypeScript
- **Angular Material** para componentes UI
- **RxJS** para programação reativa
- **NgRx** para gerenciamento de estado
- **Socket.IO** para atualizações em tempo real

#### Infraestrutura
- **Docker** e **Docker Compose** para containerização
- **PostgreSQL** 15 para dados transacionais
- **MongoDB** 6.0 para armazenamento de eventos
- **Apache Kafka** + **Zookeeper** para mensageria
- **Prometheus** + **Grafana** para monitoramento
- **ELK Stack** para logging centralizado

### 📋 Pré-requisitos
Docker 24.0+

Docker Compose 2.20+

Java 17 (apenas para desenvolvimento)

Node.js 18+ e npm (apenas para desenvolvimento Angular)

### Git

🚀 Executando o Projeto
1. Clone o repositório
```   bash
   git clone https://github.com/seu-usuario/saga-orchestration-system.git
   cd saga-orchestration-system
```
2. Inicie a infraestrutura com Docker Compose
```   bash
   docker-compose up -d
```
Este comando irá iniciar:

PostgreSQL (serviços e orquestrador)

MongoDB

Apache Kafka + Zookeeper

Prometheus + Grafana

Elasticsearch + Logstash + Kibana (opcional)

3. Execute os microserviços
```   bash
# Build dos projetos
./mvnw clean package -DskipTests

# Executar o orquestrador
java -jar orchestrator/target/orchestrator-1.0.0.jar

# Executar os serviços em terminais separados
java -jar order-service/target/order-service-1.0.0.jar
java -jar payment-service/target/payment-service-1.0.0.jar
java -jar inventory-service/target/inventory-service-1.0.0.jar
java -jar notification-service/target/notification-service-1.0.0.jar
```
4. Execute o frontend Angular
```   bash
   cd frontend
   npm install
   ng serve
   📁 Estrutura do Projeto
   text
   saga-orchestration-system/
   ├── orchestrator/              # Serviço orquestrador de sagas
   ├── order-service/            # Serviço de pedidos
   ├── payment-service/          # Serviço de pagamentos
   ├── inventory-service/        # Serviço de estoque
   ├── notification-service/     # Serviço de notificações
   ├── api-gateway/             # Gateway API
   ├── frontend/                # Aplicação Angular
   ├── docker-compose.yml       # Configuração Docker
   ├── prometheus/              # Configuração do Prometheus
   ├── grafana/                 # Dashboards do Grafana
   └── kafka/                   # Configurações do Kafka
   ⚙️ Configuração
   Variáveis de Ambiente
   Crie um arquivo .env na raiz do projeto:

env
# Banco de Dados
POSTGRES_USER=admin
POSTGRES_PASSWORD=secret
POSTGRES_DB=saga_db

# MongoDB
MONGO_INITDB_ROOT_USERNAME=admin
MONGO_INITDB_ROOT_PASSWORD=secret

# Kafka
KAFKA_BROKER_ID=1
KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181

# Aplicação
SPRING_PROFILES_ACTIVE=docker
Configuração do Kafka para Saga
yaml
# application-saga.yml
saga:
topics:
orchestration: saga-orchestration-events
compensation: saga-compensation-events
commands: saga-command-events
retry:
max-attempts: 3
backoff-delay: 1000
timeout:
saga-execution: 30000
step-execution: 10000
```
### 🔍 Monitoramento
Acesse os dashboards:

Grafana: http://localhost:3000

Kibana: http://localhost:5601 (se habilitado)

Zipkin: http://localhost:9411

Spring Boot Admin: http://localhost:9090

### 🧪 Testando Sagas
1. Criar uma nova saga via API:
```   bash
   curl -X POST http://localhost:8080/api/sagas \
   -H "Content-Type: application/json" \
   -d '{
   "type": "CREATE_ORDER",
   "payload": {
   "orderId": "12345",
   "customerId": "cust-001",
   "items": [
   {"productId": "prod-001", "quantity": 2}
   ],
   "totalAmount": 199.99
   }
   }'
```
2. Monitorar o status:
```   bash
   curl http://localhost:8080/api/sagas/12345/status
```
### 🧩 Padrão Saga Implementado
O sistema implementa o padrão Saga Orquestrado com as seguintes etapas:

Iniciação: Cliente inicia uma saga através do orquestrador

Coordenação: Orquestrador envia comandos sequenciais aos serviços

Execução: Cada serviço executa sua transação local

Compensação: Em caso de falha, comandos de compensação são executados

Finalização: Saga é marcada como completa ou compensada

### 🤝 Contribuindo
Faça um Fork do projeto

Crie uma branch para sua feature (git checkout -b feature/AmazingFeature)

Commit suas mudanças (git commit -m 'Add some AmazingFeature')

Push para a branch (git push origin feature/AmazingFeature)

Abra um Pull Request

### 📄 Licença
Este projeto está licenciado sob a licença MIT. Veja o arquivo LICENSE para mais detalhes.

### 📞 Suporte
Para suporte, abra uma issue no GitHub ou entre em contato através do email: josiassantos1577@gmail.com