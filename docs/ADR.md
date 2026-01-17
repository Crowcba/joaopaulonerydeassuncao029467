# Justificativas Técnicas - Decisões do Projeto
## Sistema de Gerenciamento de Artistas e Álbuns - PSS/SEPLAG

---

## 1. Backend

### 1.1 Java 21
- Por ser já usado há certo tempo, o risco para projeto ágil é menor que a versão 25 recém lançada. Ela possui muita compatibilidade com outras ferramentas; o 25 teria forte possibilidade de incompatibilidade e teria que fazer testes, coisa que o prazo não me permite.
### 1.2 Spring Boot 4.0.1
- A escolha foi baseada por ser uma ferramenta já consolidada no mercado, e a versão 4.0.1 já está estável desde Novembro, já tem suporte nativo para o Java 21, já tem integração com todos os requisitos do edital, assim tornando o desenvolvimento mais ágil e fácil, como você verá nos próximos itens.
### 1.3 Maven
- Ferramenta já bastante utilizada, já tem suporte nativo no Spring Initializr, resolve dependências transitivas automaticamente e ainda evita conflitos de versões, integra totalmente com Docker.
- Por já ter escolhido o Spring Boot, a escolha do Maven é praticamente natural.
### 1.4 PostgreSQL 15 (Alpine)
- Versão estável e madura, imagem leve no Docker, porém robusta, com excelente suporte a relacionamentos complexos e integração nativa com o Spring Boot.
### 1.5 JPA/Hibernate
- Tem integração nativa com o Spring Boot, redução de código repetitivo, desta forma economizando tempo de programação, tem suporte para relacionamentos complexos.
### 1.6 Flyway Migrations
- Além de ser obrigatório pelo edital, ele versiona o schema do banco de dados, evitando problemas de ambiente, também possui integração com o Spring Boot.
### 1.7 SpringDoc OpenAPI (Swagger)
- Outro item também obrigatório pelo edital, ele é o padrão para o Spring Boot. Gera documentação de modo automático a partir do código, desta forma se mantendo sempre atualizado, a interface permite testes interativos.
### 1.8 Spring Initializr (https://start.spring.io/)
- Spring Initializr garante que todas as dependências (Spring Web, JPA, PostgreSQL, Actuator, Security) estejam com versões compatíveis, evitando conflitos e gerando economia de tempo.
---

## 2. Frontend
### 2.1 Angular 20
- Versão LTS mais recente, a versão 21 ainda está como TBD.
- Angular oferece BehaviorSubject nativo via RxJS, como solicitado no edital.
- Mesmo tendo mais domínio do React, a escolha do Angular foi puramente estratégica para ganhar tempo nos itens já mencionados anteriormente, evitando problemas de compatibilidade.
### 2.2 TypeScript
- Requisito do edital.
- Reduz bugs por ser safety first, detectando erros antes da execução do código.
- Integração nativa com o Angular.
### 2.3 Node.js 24.X
- A versão mais recente com LTS.
### 2.4 Tailwind CSS (prioridade)
- Requisito do edital.
- Oferece classes utility-first que acelera o desenvolvimento de interfaces.
- Responsivo nativamente.
- Remove classes não usadas em produção.
### 2.5 RxJS com BehaviorSubject (nativo do Angular)
- Requisito do edital.
- Nativo do Angular. 
### 2.6 Facade Pattern (implementado via Services do Angular)
- Requisito do edital.
- Implementado via services do Angular.
- Abstrai a complexidade do HTTP e centraliza a lógica do negócio.  
### 2.7 Angular Router com Lazy Loading de módulos
- Requisito do edital.
- Melhoria de performance no projeto, carregando módulos somente quando necessário, reduzindo o bundle inicial.
- Nativo do Angular como Angular Router via loadChildren.
### 2.8 HttpClient (nativo do Angular)
- Nativo do Angular, facilita a integração com o RxJS e BehaviorSubject.
- Integração perfeita com o Service.
### 2.9 Jasmine + Karma (padrão Angular)
- Jasmine + Karma é padrão do Angular e pré-configurado no Angular CLI.
- Facilita testes de componentes, services e pipes com TestBed integrado.
- Suporte a cobertura de código e relatórios, atende o requisito de testes unitários.
---

## 3. Infraestrutura
### 3.1 Docker Compose
- Requisito do edital.
- Garante consistência entre ambientes e elimina a possibilidade "Funciona na minha máquina e não funciona no servidor".
- Facilita gerenciamento de múltiplos serviços.
### 3.2 MinIO
- Requisito do edital.
- Integração nativa com o Spring Boot via AWS SDK, tornando a implementação de download e upload de capas mais simples.
- Suporte a presigned URLs, requisito do edital, com expiração em 30 minutos.
---

## 4. Configuração e Variáveis de Ambiente
### 4.1 Utilização de arquivo .env commitado
- para fiz de avaliação nao teremos .env.local somente o .env que será  enviado par ao banco de dados para fins de avaliação juntamente com o projeto
