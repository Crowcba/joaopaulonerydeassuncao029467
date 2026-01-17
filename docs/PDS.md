# Plano de Desenvolvimento - PSS/SEPLAG
## Sistema de gestão de artistas e álbuns 

---

## 1. Informações
### 1.1 Dados do Edital
- **Edital:** Processo Seletivo Simplificado Nº 001/2026/SEPLAG
- **Prazo Final:** 05/02/2026
- **Repositório:** joaopaulonerydeassuncao029467
- **Perfil:** Full Stack Sênior
- **Objetivo:** 60 pontos

### 1.2 Objetivos

Implementar sistema completo de gerenciamento de artistas e álbuns com:

**Funcionalidades Básicas (Obrigatórias):**
- CRUD de Artistas (POST, PUT, GET)
- CRUD de Álbuns (POST, PUT, GET)
- Sistema de upload e recuperação de imagens das capas dos álbuns via MinIO com presigned URLs (expiração de 30 minutos) 
- Sistema de autenticação JWT com expiração de 5 minutos e renovação refresh token
- Consultas com filtros e ordenação (asc/desc)
- Paginação nas telas de consultas de álbuns e artistas
- Versionamento de endpoints como "/api/v1/"
- Documentação de endpoints via Swagger/OpenAPI



**Funcionalidades Sênior (Requisitos Adicionais):**

- Notificações em tempo real utilizando WebSocket a cada novo álbum cadastrado
- Integração com API externa com sincronização (https://integrador-argus-api.geia.vip/v1/regionais)
- Health Checks (Liveness e Readiness)
- Rate Limit de 10 requisições por minuto por usuário
- Testes unitários obrigatórios

*Referência completa dos requisitos: [requisitos_edital.md](requisitos_edital.md)*

### 1.3 Exemplos Dados como Referência
- Serj Tankian → "Harakiri", "Black Blooms", "The Rough Dog"
- Mike Shinoda → "The Rising Tied", "Post Traumatic", "Post Traumatic EP", "Where'd You Go"
- Michel Teló → "Bem Sertanejo", "Bem Sertanejo - O Show (Ao Vivo)", "Bem Sertanejo - (1ª Temporada) - EP"
- Guns N' Roses → "Use Your Illusion I", "Use Your Illusion II", "Greatest Hits"

## 2. Ferramentas e Equipamentos Utilizados no Projeto
### 2.1 Ferramentas 
- **Sistema Operacional:** Windows 11 Pro
- **NOTA:** *Optei por não usar o WSL para este projeto por motivos pessoais, não migrei para Linux por ser meu computador do dia a dia*
- **PowerShell:** Bom terminal já nativo do Windows, outras opções são: Warp e Cmder.
- **VSCode:**  Programa padrão para codar.
- **Docker:**  Orquestração de containers.
- **lucid.app:**  Documentação de banco de dados.
- **Word:** Para observações e rascunhos. 

### 2.2 Equipamento Utilizado
- Dell Precision 3581
- 32 GB de RAM DDR5
- NVIDIA RTX ADA 2000 8GB GDDR6
- NVME de 1 TB


## 3. Stack Tecnológica e Decisões Técnicas

### 3.1 Backend
- **Framework:** Spring Boot
- **Versão:** Spring Boot 4.0.1
- **Linguagem:** Java 21
- **Build Tool**: Maven
- **Banco de Dados:** PostgreSQL 15 (Alpine)
- **ORM/JPA:** JPA/Hibernate
- **Migrations:** Flyway
- **Documentação API:** SpringDoc OpenAPI (Swagger)
- **Ferramenta de Inicialização:** Spring Initializr (https://start.spring.io/)

### 3.2 Frontend
- **Framework:** Angular
- **Versão:** Angular 20
- **Linguagem:** TypeScript
- **Node.js:** Node.js 24.12
- **CSS Framework:** Tailwind CSS (prioridade)
- **Gerenciamento de Estado:** RxJS com BehaviorSubject (nativo do Angular)
- **Padrão Arquitetural:** Facade Pattern (implementado via Services do Angular)
- **Roteamento:** Angular Router com Lazy Loading de módulos
- **HTTP Client:** HttpClient (nativo do Angular)
- **Testes:** Jasmine + Karma (padrão Angular)

### 3.3 Infraestrutura
- **Orquestração:** Docker Compose
- **Armazenamento:** MinIO (API S3)
- **Versão MinIO:** latest
- **Network:** seplag-network (bridge driver)
- **Containers:**
  - Backend (API - Spring Boot 4.0.1)
  - Frontend (Angular 20)
  - Banco de Dados (PostgreSQL 15-alpine)
  - MinIO (latest)

### 3.4 Segurança e Autenticação
- **JWT:** Biblioteca jjwt (io.jsonwebtoken:jjwt)
- **Expiração Token:** 5 minutos
- **Refresh Token:** Implementar renovação
- **CORS:** Configurar para bloquear domínios externos

### 3.5 Requisitos Sênior
- **Health Checks:** Spring Boot Actuator
- **Rate Limiting:** Bucket4j
- **WebSocket:** Spring WebSocket
- **Testes:** JUnit 5 + Mockito

### 3.6 Integração Externa
- **Abordagem:** Monólito Modular
    - Pacote isolado
    - Execução assíncrona
    - Cliente HTTP WebClient (Spring)
- **Endpoint:** https://integrador-argus-api.geia.vip/v1/regionais

## 4. Estrutura do Projeto e Arquitetura

### 4.1 Estrutura de Pastas
```
backend/
    src/
        main/
            java/
                com/
                    seplag/
                        musica/
                            controller/
                            service/
                            repository/
                            model/
                            dto/
                            config/
                            security/
                            exception/
            resources/
                application.properties
                db/
                    migration/
        test/
    pom.xml
    Dockerfile

frontend/
    src/
        app/
            core/
            features/
                artistas/
                albuns/
                auth/
            shared/
            layout/
        assets/
        environments/
    angular.json
    Dockerfile

/
    backend/
    frontend/
    docs/
    trash/
    src/
    docker-compose.yml
    README.md
    .gitignore
```
**Nota:**
    - Pasta trash estaria no .gitignore, mas para fins de avaliação ela será mantida no git. 

### 4.2 Arquitetura do Sistema

**Padrão Arquitetural:**
- **Backend:** Clean Architecture (Controller → Service → Repository → Model)
- **Frontend:** Feature Modules com Lazy Loading
- **Comunicação:** REST API (JSON)
- **Autenticação:** JWT Stateless
- **Armazenamento:** MinIO (S3-compatible)

**Fluxo de Dados:**
1. Frontend (Angular) → HTTP Client → Backend (Spring Boot)
2. Backend → JPA/Hibernate → PostgreSQL
3. Backend → MinIO Client → MinIO (Imagens)
4. Backend → WebSocket → Frontend (Notificações)

### 4.3 Modelo de Dados

**Entidades Principais:**
- **Artista** (id, nome, dataCriacao, dataAtualizacao)
- **Album** (id, nome, artistaId, dataLancamento, dataCriacao, dataAtualizacao)
- **AlbumCapa** (id, albumId, nomeArquivo, url, dataUpload)
- **Regional** (id, nome, ativo, dataCriacao, dataAtualizacao)

**Relacionamentos:**
- Artista 1:N Album
- Album 1:N AlbumCapa
- Regional (independente - sincronização externa)

## 5. Cronograma e Acompanhamento de Desenvolvimento

**Tarefas:**
- **Fase:** Análise e Design.
- **Início:** 15/01
- **Data Commit:** 17/01
- **Descrição:** Documentação | Estrutura básica
    - [x] Leitura do edital.
    - [x] Planejamento manual - documentação escrita.
    - [x] Documento com requisitos do edital -> Guia do projeto.
    - [x] Criar repositório GitHub.(https://github.com/Crowcba/joaopaulonerydeassuncao029467)
    - [x] Configurar estrutura de pastas. (backend, frontend, docs)
    - [x] Criar plano de desenvolvimento.
    - [x] Criar Arquivo .env
    - [x] Criar o arquivo Docker com o básico do sistema.
    - [x] Iniciar o container do Docker do projeto.
    - [x] Verificar o healthcheck.
        - [x] health: postgres tempo 11 segundos
        - [x] health: minio tempo 32 segundos
    - [x] Planejar diagramaER de banco de dados.
    - [x] Planejar estrutura de banco de dados.
    - [x] Criar documentação do banco de dados base para backend e frontend do projeto.
- **Observações:**
    Leitura e planejamento inicial. A leitura apontou uma coisa interessante: da opção de React e Angular, porém escolher o React deixa o projeto mais demorado, pois muitos requisitos do edital já são nativos do Angular. Com uma pequena leitura no edital e uma pesquisa, qualquer um conseguiria definir isso.

    As senhas escolhidas para o projeto foram simples para ambiente de desenvolvimento, dessa forma, para evitar conflitos com caracteres especiais nesse início do projeto.

