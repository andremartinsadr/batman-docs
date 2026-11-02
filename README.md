# Documentação do Batman

Documentação técnica do projeto Batman, incluindo a arquitetura da aplicação, a API de comentários e os diagramas UML e DER.

## Repositórios

- Aplicação: [andremartinsadr/batman-app](https://github.com/andremartinsadr/batman-app)
- Documentação: [andremartinsadr/batman-docs](https://github.com/andremartinsadr/batman-docs)

## Tecnologias

- Node.js e Express para o servidor e a API REST.
- HTML, CSS e JavaScript para o site estático.
- MySQL para persistência dos comentários.

## Executar o projeto

1. Instale o Node.js e o MySQL.
2. Clone o repositório da aplicação e instale as dependências:

   ```sh
   git clone https://github.com/andremartinsadr/batman-app.git
   cd batman-app
   npm install
   ```

3. Crie o banco e a tabela executando [`schema.sql`](https://github.com/andremartinsadr/batman-app/blob/main/src/config/schema.sql) no MySQL.
4. Copie `.env.example` para `.env` e ajuste as credenciais do banco conforme seu ambiente.
5. Inicie o servidor:

   ```sh
   npm start
   ```

6. Acesse <http://localhost:3000>.

O arquivo `.env` contém configurações locais e não deve ser compartilhado.

## API de comentários

As rotas são montadas sob o prefixo `/api`.

### `GET /api/comentarios`

Retorna os comentários em ordem decrescente de data e identificador. Os campos retornados são `id`, `nome`, `mensagem`, `nota` e `data_criacao`; o e-mail não é exposto nessa listagem.

### `POST /api/comentarios`

Recebe JSON com `nome`, `email`, `mensagem` e `nota`.

```json
{
  "nome": "Bruce Wayne",
  "email": "bruce@example.com",
  "mensagem": "Gotham precisa de esperança.",
  "nota": 5
}
```

A nota deve ser um número inteiro de 1 a 5. Campos de texto não podem estar vazios e o e-mail deve ter formato válido. Respostas: `201` ao gravar, `400` para entrada inválida e `500` em erro interno.

## Estrutura

```text
server.js                         Inicialização do Express e arquivos estáticos
src/
  config/
    database.js                   Pool de conexão MySQL
    schema.sql                    Banco e tabela para instalação nova
  controllers/
    ComentarioController.js       Validação e respostas HTTP
  models/
    Comentario.js                 Consultas SQL de comentários
  routes/
    routes.js                     Rotas da API
public/                           Páginas, estilos, imagens e scripts do site
```

Os diagramas e detalhes da modelagem estão em [`docs/modelagem.md`](docs/modelagem.md).

## Relatório técnico

O [relatório técnico do projeto](Relatorio_Tecnico.md) reúne os diagramas de casos de uso, classes e entidade-relacionamento, além de capturas das páginas e modais em execução. Também está disponível para [download em .docx](Relatorio_Tecnico_Projeto_Batman.docx). Os arquivos-fonte dos diagramas e as capturas estão em [`assets/`](assets/).
