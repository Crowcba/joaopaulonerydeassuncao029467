# Modelo de Dados - Sistema de Gerenciamento de Artistas e Álbuns

## 1. Visão Geral

O banco de dados foi modelado seguindo a Terceira Forma Normal (3NF), garantindo integridade referencial e minimização de redundâncias.

**Total de Tabelas:** 7
- `usuario` - Gerenciamento de usuários e autenticação
- `artista` - Cadastro de artistas musicais
- `album` - Cadastro de álbuns
- `album_capa` - Metadados de capas de álbuns (integração com MinIO)
- `log` - Auditoria e rastreamento de ações
- `refresh_token` - Tokens de renovação JWT
- `regional` - Dados regionais (sincronização externa)

---

## 2. Tabelas Detalhadas

### 2.1 Tabela: usuario

**Descrição:** Armazena informações de usuários do sistema, incluindo credenciais de autenticação e controle de acesso.

**Campos:**
| Campo | Tipo | Constraint | Nullable | Default | Descrição |
|-------|------|------------|----------|---------|-----------|
| id | BIGSERIAL | PK | NOT NULL | - | Identificador único |
| email | VARCHAR(150) | UK | NOT NULL | - | E-mail único do usuário |
| nome | VARCHAR(100) | - | NOT NULL | - | Nome completo |
| role | VARCHAR(20) | - | NULL | - | Papel/perfil do usuário |
| senha | VARCHAR(80) | - | NOT NULL | - | Senha criptografada |
| admin | BOOLEAN | - | - | FALSE | Indica se é administrador |
| erro_login | INTEGER | - | NULL | - | Contador de erros de login |
| ultimo_login | TIMESTAMP | - | NULL | - | Data/hora do último login |
| habilitado | BOOLEAN | - | - | TRUE | Indica se o usuário está habilitado |
| ativo | BOOLEAN | - | - | TRUE | Soft delete |

**Relacionamentos:**
- 1:N com `refresh_token` (um usuário pode ter múltiplos tokens)
- 1:N com `log` (um usuário pode gerar múltiplos logs)

**Índices:**
- PK: `id`
- UK: `email`

---

### 2.2 Tabela: artista

**Descrição:** Armazena informações sobre artistas musicais.

**Campos:**
| Campo | Tipo | Constraint | Nullable | Default | Descrição |
|-------|------|------------|----------|---------|-----------|
| id | BIGSERIAL | PK | NOT NULL | - | Identificador único |
| nome | VARCHAR(100) | - | NOT NULL | - | Nome do artista |
| biografia | TEXT | - | NULL | - | Biografia do artista |
| criado_em | TIMESTAMP | - | NOT NULL | - | Data/hora de criação |
| criado_por | VARCHAR(100) | - | NOT NULL | - | Usuário que criou |
| alterado_em | TIMESTAMP | - | NULL | - | Data/hora da última alteração |
| alterado_por | VARCHAR(100) | - | NULL | - | Usuário que alterou |
| ativo | BOOLEAN | - | - | TRUE | Soft delete |

**Relacionamentos:**
- 1:N com `album` (um artista pode ter múltiplos álbuns)

**Índices:**
- PK: `id`

---

### 2.3 Tabela: album

**Descrição:** Armazena informações sobre álbuns musicais, relacionados a um artista.

**Campos:**
| Campo | Tipo | Constraint | Nullable | Default | Descrição |
|-------|------|------------|----------|---------|-----------|
| id | BIGSERIAL | PK | NOT NULL | - | Identificador único |
| titulo | VARCHAR(100) | - | NOT NULL | - | Título do álbum |
| ano_lancamento | INTEGER | - | NOT NULL | - | Ano de lançamento |
| artista_id | BIGSERIAL | FK → artista.id | NOT NULL | - | Referência ao artista |
| criado_em | TIMESTAMP | - | NOT NULL | - | Data/hora de criação |
| criado_por | VARCHAR(100) | - | NOT NULL | - | Usuário que criou |
| alterado_em | TIMESTAMP | - | NULL | - | Data/hora da última alteração |
| alterado_por | VARCHAR(100) | - | NULL | - | Usuário que alterou |
| ativo | BOOLEAN | - | - | TRUE | Soft delete |

**Relacionamentos:**
- N:1 com `artista` (múltiplos álbuns pertencem a um artista)
- 1:N com `album_capa` (um álbum pode ter múltiplas capas)

**Índices:**
- PK: `id`
- FK: `artista_id` → `artista.id`

---

### 2.4 Tabela: album_capa

**Descrição:** Armazena metadados das capas de álbuns armazenadas no MinIO.

**Campos:**
| Campo | Tipo | Constraint | Nullable | Default | Descrição |
|-------|------|------------|----------|---------|-----------|
| id | BIGSERIAL | PK | NOT NULL | - | Identificador único |
| nome | VARCHAR(100) | - | NOT NULL | - | Nome do arquivo |
| album_id | BIGSERIAL | FK → album.id | NOT NULL | - | Referência ao álbum |
| chave_arquivo | VARCHAR(50) | - | NOT NULL | - | Chave do arquivo no MinIO |
| tipo_arquivo | VARCHAR(10) | - | NOT NULL | - | Tipo MIME (ex: image/jpeg) |
| criado_em | TIMESTAMP | - | NOT NULL | - | Data/hora de criação |
| criado_por | VARCHAR(100) | - | NULL | - | Usuário que criou |
| alterado_em | TIMESTAMP | - | NULL | - | Data/hora da última alteração |
| alterado_por | VARCHAR(100) | - | NULL | - | Usuário que alterou |
| ativo | BOOLEAN | - | - | TRUE | Soft delete |

**Relacionamentos:**
- N:1 com `album` (múltiplas capas pertencem a um álbum)

**Índices:**
- PK: `id`
- FK: `album_id` → `album.id`

---

### 2.5 Tabela: log

**Descrição:** Registra ações dos usuários para auditoria e rastreamento.

**Campos:**
| Campo | Tipo | Constraint | Nullable | Default | Descrição |
|-------|------|------------|----------|---------|-----------|
| id | BIGSERIAL | PK | NOT NULL | - | Identificador único |
| usuario_id | BIGSERIAL | FK → usuario.id | NOT NULL | -