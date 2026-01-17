Prazo Final -> pag. 4 | item: 6.2.2.2 -> 05/02/2026 (data limite para commits e alterações no GitHub).

Identificação do Repositório -> pag. 4 | item: 6.2.2.1 -> nomecompleto + 06 primeiros dígitos do CPF (Ex: joaopaulonerydeassuncao029467).

Objetivo: Implementar solução Full Stack para gerenciamento de artistas e álbuns (Exemplos: Serj Tankian, Mike Shinoda, Michel Teló, Guns N' Roses).

Requisitos Técnicos - Full Stack

Backend -> pag. 12 | Anexo II-C -> Java com Spring Boot ou Quarkus. Implementar verbos POST, PUT, GET e versionamento de endpoints.

Frontend -> pag. 12 | Anexo II-C -> React ou Angular com TypeScript. Interface intuitiva, responsiva, com uso preferencial de Tailwind CSS.

Banco de Dados -> pag. 12 | Anexo II-C -> Estrutura de dados coerente para tabelas. Uso obrigatório de Flyway Migrations para criar e popular o banco.

Armazenamento -> pag. 12 | Anexo II-C -> Uso de MinIO (API S3) para imagens de capa. Recuperação via links pré-assinados (presigned URL) com expiração de 30 minutos.

Monitoramento -> pag. 13 | Anexo II-C -> Endpoints de Health Checks, Liveness e Readiness (Requisito Sênior).

Segurança (API) -> pag. 12 | Anexo II-C -> Bloqueio de domínios distintos do serviço (CORS). Autenticação JWT com expiração de 5 minutos e suporte a renovação (refresh token).

API Rate Limit -> pag. 13 | Anexo II-C -> Implementar limite de no máximo 10 requisições por minuto por usuário (Requisito Sênior).

Real-time -> pag. 13 | Anexo II-C -> WebSocket na API com notificações no Frontend a cada novo álbum cadastrado (Requisito Sênior).

Integração Externa (Sênior) -> pag. 13 | Anexo II-C -> Sincronizar dados do endpoint de regionais (https://integrador-argus-api.geia.vip/v1/regionais) com menor complexidade algorítmica: inserir novos, inativar ausentes e tratar alterações de atributos inativando o registro anterior.

Padrões de Código (Exigência Sênior) -> pag. 13 | Anexo II-C -> Frontend com Padrão Facade e gestão de estado via BehaviorSubject. Aplicar Clean Code e modularização.

Testes Unitários -> pag. 13 | Anexo II-C -> Implementação obrigatória de testes unitários (Requisito Sênior).

Orquestração Docker -> pag. 12 | Anexo II-C -> Entrega via docker-compose subindo API + MinIO + Banco de Dados + Frontend.

Documentação (README.md) -> pag. 13 | Anexo II-C -> Documentação da arquitetura, dados de inscrição e instruções de execução/testes. Documentar endpoints com OpenAPI/Swagger.

Experiência e Commits -> pag. 13 | Anexo II-C -> Histórico de commits pequenos e organizados; código autoral e soluções simples e práticas.

OBJETIVO -> 60 pontos.