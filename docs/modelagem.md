# Arquitetura e modelagem

## Arquitetura da aplicação

O navegador recebe as páginas e os arquivos estáticos servidos pelo Express. As operações de comentários usam a API REST, que encaminha as requisições ao controller, ao model e ao banco MySQL.

```mermaid
flowchart LR
    browser["Navegador"]
    server["Express<br/>server.js"]
    pages["Arquivos estáticos<br/>public/"]
    routes["Rotas REST<br/>src/routes/routes.js"]
    controller["Controller<br/>ComentarioController"]
    model["Model<br/>Comentario"]
    db["MySQL<br/>batman_db"]

    browser -->|"GET páginas e arquivos"| server
    server --> pages
    browser -->|"GET/POST /api/comentarios"| server
    server --> routes
    routes --> controller
    controller --> model
    model --> db
```

## Diagrama de classes (UML)

```mermaid
classDiagram
    class ExpressServer {
        +servirArquivosEstaticos()
        +montarRotasAPI()
    }
    class ComentarioRoutes {
        +GET_comentarios()
        +POST_comentarios()
    }
    class ComentarioController {
        +criarComentario(request, response)
        +obterComentarios(request, response)
    }
    class Comentario {
        +salvar(nome, email, mensagem, nota)$
        +listarTodos()$
    }
    class MySQLDatabase {
        +execute(sql, parametros)
    }

    ExpressServer --> ComentarioRoutes : monta em /api
    ComentarioRoutes --> ComentarioController : encaminha
    ComentarioController --> Comentario : valida e consulta
    Comentario --> MySQLDatabase : persiste e lista
```

## Diagrama entidade-relacionamento (DER)

O esquema atual possui uma entidade. Cada comentário guarda nome, e-mail, mensagem, nota e data de criação. Não há relacionamentos com outras entidades.

```mermaid
erDiagram
    COMENTARIOS {
        INT id PK "AUTO_INCREMENT"
        VARCHAR nome "100, obrigatório"
        VARCHAR email "100, obrigatório"
        TEXT mensagem "obrigatório"
        TINYINT nota "1 a 5, obrigatório"
        TIMESTAMP data_criacao "padrão CURRENT_TIMESTAMP"
    }
```

## Regras e persistência

- O banco é `batman_db`, criado em `utf8mb4`.
- `id` é a chave primária autoincrementada.
- `nota` é um inteiro sem sinal e o controller limita o valor de 1 a 5.
- A criação grava `nome`, `email`, `mensagem` e `nota`.
- A listagem ordena por `data_criacao` e depois por `id`, ambos em ordem decrescente, e não inclui o e-mail na resposta.
- O model usa consultas parametrizadas para gravar dados.
- Para uma instalação nova, `src/config/schema.sql` é a referência do esquema atual. A migration `001-add-rating.sql` é uma alteração histórica para uma tabela anterior; não execute essa migration depois do esquema atual, pois ele já contém a coluna `nota`.
