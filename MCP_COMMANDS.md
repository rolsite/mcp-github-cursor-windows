# Documentação de Comandos MCP GitHub

Este documento fornece uma referência completa para todos os comandos disponíveis no MCP GitHub para o Cursor IDE.

## Índice

- [Configuração](#configuração)
- [Comandos de Repositório](#comandos-de-repositório)
- [Comandos de Branches](#comandos-de-branches)
- [Comandos de Arquivos](#comandos-de-arquivos)
- [Comandos de Issues](#comandos-de-issues)
- [Comandos de Pull Requests](#comandos-de-pull-requests)
- [Comandos de Revisão de Código](#comandos-de-revisão-de-código)
- [Comandos de Busca](#comandos-de-busca)
- [Comandos de Segurança](#comandos-de-segurança)
- [Cursor Rules para MCP GitHub](#cursor-rules-para-mcp-github)

## Configuração

Antes de usar os comandos MCP GitHub, configure o MCP Server conforme descrito no [README.md](README.md).

## Comandos de Repositório

### create_repository

Cria um novo repositório no GitHub.

**Parâmetros:**
- `name` (obrigatório): Nome do repositório
- `description`: Descrição do repositório
- `private`: Booleano indicando se o repositório é privado
- `autoInit`: Booleano indicando se deve inicializar com README

**Exemplo:**
```json
{
  "name": "meu-novo-repo",
  "description": "Um repositório criado via MCP",
  "private": false,
  "autoInit": true
}
```

### fork_repository

Faz fork de um repositório existente para sua conta.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório original
- `repo` (obrigatório): Nome do repositório original
- `organization`: Nome da organização para a qual fazer o fork

**Exemplo:**
```json
{
  "owner": "github",
  "repo": "github-mcp-server",
  "organization": "minha-org"
}
```

### get_me

Obtém informações sobre o usuário autenticado.

**Parâmetros:**
- `reason`: Razão opcional pela qual a sessão foi criada

**Exemplo:**
```json
{}
```

## Comandos de Branches

### create_branch

Cria um novo branch em um repositório.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `branch` (obrigatório): Nome do novo branch
- `from_branch`: Branch de origem (padrão: branch padrão do repositório)

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "branch": "feature/nova-funcionalidade",
  "from_branch": "main"
}
```

### list_branches

Lista branches em um repositório.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `page`: Número da página para paginação
- `perPage`: Resultados por página

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows"
}
```

## Comandos de Arquivos

### create_or_update_file

Cria ou atualiza um único arquivo em um repositório.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `path` (obrigatório): Caminho do arquivo
- `content` (obrigatório): Conteúdo do arquivo
- `message` (obrigatório): Mensagem de commit
- `branch` (obrigatório): Branch para criar/atualizar o arquivo
- `sha`: SHA do arquivo sendo substituído (para atualizações)

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "path": "docs/tutorial.md",
  "content": "# Tutorial\n\nAqui está um tutorial...",
  "message": "docs: adiciona arquivo de tutorial",
  "branch": "main"
}
```

### get_file_contents

Obtém o conteúdo de um arquivo ou diretório.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `path` (obrigatório): Caminho para o arquivo/diretório
- `branch`: Branch de onde obter o conteúdo

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "path": "README.md",
  "branch": "main"
}
```

### push_files

Envia múltiplos arquivos para um repositório em um único commit.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `branch` (obrigatório): Branch para enviar
- `files` (obrigatório): Array de objetos de arquivo, cada um com `path` e `content`
- `message` (obrigatório): Mensagem de commit

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "branch": "main",
  "files": [
    {
      "path": "docs/guia.md",
      "content": "# Guia\n\nConteúdo do guia..."
    },
    {
      "path": "docs/reference.md",
      "content": "# Referência\n\nConteúdo da referência..."
    }
  ],
  "message": "docs: adiciona arquivos de documentação"
}
```

## Comandos de Issues

### create_issue

Cria uma nova issue em um repositório.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `title` (obrigatório): Título da issue
- `body`: Conteúdo da issue
- `assignees`: Array de nomes de usuários para atribuir à issue
- `labels`: Array de labels para aplicar à issue
- `milestone`: Número do milestone

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "title": "Adicionar documentação para novos comandos",
  "body": "Precisamos adicionar documentação para os novos comandos MCP.",
  "assignees": ["rolsite"],
  "labels": ["documentation", "enhancement"]
}
```

### get_issue

Obtém detalhes de uma issue específica.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `issue_number` (obrigatório): Número da issue

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "issue_number": 1
}
```

### add_issue_comment

Adiciona um comentário a uma issue existente.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `issue_number` (obrigatório): Número da issue
- `body` (obrigatório): Conteúdo do comentário

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "issue_number": 1,
  "body": "Estou trabalhando nesta documentação agora."
}
```

### get_issue_comments

Obtém comentários de uma issue.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `issue_number` (obrigatório): Número da issue
- `page`: Número da página
- `per_page`: Resultados por página

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "issue_number": 1
}
```

### list_issues

Lista issues em um repositório.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `state`: Estado das issues (open, closed, all)
- `labels`: Filtrar por labels
- `sort`: Ordenar por (created, updated, comments)
- `direction`: Direção da ordenação (asc, desc)
- `since`: Filtrar por data
- `page`: Número da página
- `perPage`: Resultados por página

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "state": "open",
  "labels": ["bug"]
}
```

### update_issue

Atualiza uma issue existente.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `issue_number` (obrigatório): Número da issue a atualizar
- `title`: Novo título
- `body`: Nova descrição
- `state`: Novo estado (open, closed)
- `assignees`: Novos responsáveis
- `labels`: Novas labels
- `milestone`: Novo milestone

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "issue_number": 1,
  "state": "closed",
  "labels": ["documentation", "completed"]
}
```

## Comandos de Pull Requests

### create_pull_request

Cria um novo pull request.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `title` (obrigatório): Título do PR
- `head` (obrigatório): Branch contendo as mudanças
- `base` (obrigatório): Branch para mesclar
- `body`: Descrição do PR
- `draft`: Criar como rascunho
- `maintainer_can_modify`: Permitir edições do mantenedor

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "title": "Adiciona documentação completa do MCP",
  "head": "feature/mcp-docs",
  "base": "main",
  "body": "Este PR adiciona documentação completa para todos os comandos MCP."
}
```

### get_pull_request

Obtém detalhes de um pull request.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `pullNumber` (obrigatório): Número do pull request

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "pullNumber": 1
}
```

### list_pull_requests

Lista e filtra pull requests.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `state`: Filtro por estado (open, closed, all)
- `head`: Filtro por branch de origem
- `base`: Filtro por branch de destino
- `sort`: Ordenar por
- `direction`: Direção da ordenação
- `page`: Número da página
- `perPage`: Resultados por página

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "state": "open",
  "base": "main"
}
```

### get_pull_request_files

Obtém a lista de arquivos alterados em um pull request.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `pullNumber` (obrigatório): Número do pull request

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "pullNumber": 1
}
```

### update_pull_request

Atualiza um pull request existente.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `pullNumber` (obrigatório): Número do pull request
- `title`: Novo título
- `body`: Nova descrição
- `state`: Novo estado
- `base`: Novo branch base
- `maintainer_can_modify`: Permitir edições do mantenedor

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "pullNumber": 1,
  "title": "[WIP] Adiciona documentação MCP",
  "state": "open"
}
```

### merge_pull_request

Mescla um pull request.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `pullNumber` (obrigatório): Número do pull request
- `commit_title`: Título para o commit de merge
- `commit_message`: Mensagem detalhada para o commit
- `merge_method`: Método de merge (merge, squash, rebase)

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "pullNumber": 1,
  "merge_method": "squash"
}
```

### update_pull_request_branch

Atualiza um branch de pull request com as últimas mudanças do branch base.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `pullNumber` (obrigatório): Número do pull request
- `expectedHeadSha`: SHA esperado do HEAD do pull request

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "pullNumber": 1
}
```

## Comandos de Revisão de Código

### create_pull_request_review

Cria uma revisão em um pull request.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `pullNumber` (obrigatório): Número do pull request
- `event` (obrigatório): Ação de revisão (APPROVE, REQUEST_CHANGES, COMMENT)
- `body`: Texto do comentário de revisão
- `commitId`: SHA do commit para revisar
- `comments`: Array de comentários específicos de linha

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "pullNumber": 1,
  "event": "APPROVE",
  "body": "Ótimo trabalho! Aprovado."
}
```

### add_pull_request_review_comment

Adiciona um comentário de revisão a um pull request.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `pull_number` (obrigatório): Número do pull request
- `body` (obrigatório): Texto do comentário de revisão
- `path`: Caminho relativo do arquivo
- `line`: Linha do blob no diff
- `side`: Lado do diff para comentar
- `commit_id`: SHA do commit
- `in_reply_to`: ID do comentário para responder

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "pull_number": 1,
  "body": "Sugiro adicionar mais exemplos aqui.",
  "path": "README.md",
  "line": 42,
  "side": "RIGHT"
}
```

### get_pull_request_reviews

Obtém as revisões em um pull request.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `pullNumber` (obrigatório): Número do pull request

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "pullNumber": 1
}
```

### get_pull_request_comments

Obtém os comentários de revisão em um pull request.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `pullNumber` (obrigatório): Número do pull request

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "pullNumber": 1
}
```

## Comandos de Busca

### search_code

Realiza uma busca por código nos repositórios do GitHub.

**Parâmetros:**
- `q` (obrigatório): Query de busca usando a sintaxe de busca de código do GitHub
- `sort`: Campo para ordenação
- `order`: Ordem de classificação
- `page`: Número da página
- `perPage`: Resultados por página

**Exemplo:**
```json
{
  "q": "repo:rolsite/mcp-github-cursor-windows language:markdown",
  "order": "desc"
}
```

### search_repositories

Busca por repositórios no GitHub.

**Parâmetros:**
- `query` (obrigatório): Query de busca
- `page`: Número da página
- `perPage`: Resultados por página

**Exemplo:**
```json
{
  "query": "mcp-github language:typescript"
}
```

### search_issues

Busca por issues e pull requests no GitHub.

**Parâmetros:**
- `q` (obrigatório): Query de busca usando a sintaxe de busca de issues do GitHub
- `sort`: Campo para ordenação
- `order`: Ordem de classificação
- `page`: Número da página
- `perPage`: Resultados por página

**Exemplo:**
```json
{
  "q": "repo:rolsite/mcp-github-cursor-windows is:issue is:open"
}
```

### search_users

Busca por usuários no GitHub.

**Parâmetros:**
- `q` (obrigatório): Query de busca usando a sintaxe de busca de usuários do GitHub
- `sort`: Campo para ordenação
- `order`: Ordem de classificação
- `page`: Número da página
- `perPage`: Resultados por página

**Exemplo:**
```json
{
  "q": "location:brazil language:javascript"
}
```

## Comandos de Segurança

### list_code_scanning_alerts

Lista alertas de escaneamento de código em um repositório.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `state`: Filtro por estado
- `ref`: Referência Git
- `severity`: Filtro por severidade
- `tool_name`: Nome da ferramenta de escaneamento

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "state": "open"
}
```

### get_code_scanning_alert

Obtém detalhes de um alerta específico de escaneamento de código.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `alertNumber` (obrigatório): Número do alerta

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "alertNumber": 1
}
```

### list_secret_scanning_alerts

Lista alertas de escaneamento de segredos em um repositório.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `state`: Filtro por estado
- `secret_type`: Lista separada por vírgulas de tipos de segredo
- `resolution`: Filtro por resolução

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "state": "open"
}
```

### get_secret_scanning_alert

Obtém detalhes de um alerta específico de escaneamento de segredos.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `alertNumber` (obrigatório): Número do alerta

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "alertNumber": 1
}
```

## Outros Comandos

### get_commit

Obtém detalhes de um commit.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `sha` (obrigatório): SHA do commit, nome do branch ou tag
- `page`: Número da página
- `perPage`: Resultados por página

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "sha": "main"
}
```

### list_commits

Obtém a lista de commits de um branch.

**Parâmetros:**
- `owner` (obrigatório): Proprietário do repositório
- `repo` (obrigatório): Nome do repositório
- `sha`: SHA ou nome do branch
- `page`: Número da página
- `perPage`: Resultados por página

**Exemplo:**
```json
{
  "owner": "rolsite",
  "repo": "mcp-github-cursor-windows",
  "sha": "main"
}
```

## Cursor Rules para MCP GitHub

Abaixo está uma configuração completa de Cursor Rules para aproveitar ao máximo o MCP GitHub:

```json
{
  "rules": [
    {
      "name": "GitHub - Repository - Criar",
      "description": "Criar novo repositório via MCP",
      "pattern": "/criar( novo)? repo(sitório)?|new repo(sitory)?/i",
      "actions": [
        {
          "type": "command",
          "command": "mcp.invoke",
          "args": {
            "provider": "github-mcp",
            "command": "create_repository"
          }
        }
      ]
    },
    {
      "name": "GitHub - Repository - Fork",
      "description": "Fazer fork de repositório via MCP",
      "pattern": "/fork( repo(sitório)?)?/i",
      "actions": [
        {
          "type": "command",
          "command": "mcp.invoke",
          "args": {
            "provider": "github-mcp",
            "command": "fork_repository"
          }
        }
      ]
    },
    {
      "name": "GitHub - Branch - Criar",
      "description": "Criar nova branch via MCP",
      "pattern": "/criar branch|nova branch|new branch|git branch/i",
      "actions": [
        {
          "type": "command",
          "command": "mcp.invoke",
          "args": {
            "provider": "github-mcp",
            "command": "create_branch"
          }
        }
      ]
    },
    {
      "name": "GitHub - Branch - Listar",
      "description": "Listar branches via MCP",
      "pattern": "/listar branches|list branches/i",
      "actions": [
        {
          "type": "command",
          "command": "mcp.invoke",
          "args": {
            "provider": "github-mcp",
            "command": "list_branches"
          }
        }
      ]
    },
    {
      "name": "GitHub - Files - Commit",
      "description": "Fazer commit via MCP",
      "pattern": "/git commit|commit changes/i",
      "actions": [
        {
          "type": "suggest",
          "label": "Usar MCP GitHub",
          "message": "Posso realizar o commit usando MCP GitHub. Você gostaria de usar create_or_update_file ou push_files?"
        }
      ]
    },
    {
      "name": "GitHub - Files - Push",
      "description": "Fazer push via MCP",
      "pattern": "/git push|push changes/i",
      "actions": [
        {
          "type": "suggest",
          "label": "Usar MCP GitHub",
          "message": "Posso enviar as alterações usando MCP GitHub sem precisar de git push. Quer que eu faça isso?"
        }
      ]
    },
    {
      "name": "GitHub - Files - Atualizar Arquivo",
      "description": "Atualizar arquivo via MCP",
      "pattern": "/update file|atualizar arquivo/i",
      "actions": [
        {
          "type": "command",
          "command": "mcp.invoke",
          "args": {
            "provider": "github-mcp",
            "command": "create_or_update_file"
          }
        }
      ]
    },
    {
      "name": "GitHub - Files - Obter Conteúdo",
      "description": "Obter conteúdo de arquivo via MCP",
      "pattern": "/get file( content)?|obter (conteúdo de )?arquivo/i",
      "actions": [
        {
          "type": "command",
          "command": "mcp.invoke",
          "args": {
            "provider": "github-mcp",
            "command": "get_file_contents"
          }
        }
      ]
    },
    {
      "name": "GitHub - Issues - Criar",
      "description": "Criar issue via MCP",
      "pattern": "/criar issue|nova issue|new issue/i",
      "actions": [
        {
          "type": "command",
          "command": "mcp.invoke",
          "args": {
            "provider": "github-mcp",
            "command": "create_issue"
          }
        }
      ]
    },
    {
      "name": "GitHub - Issues - Listar",
      "description": "Listar issues via MCP",
      "pattern": "/listar issues|list issues/i",
      "actions": [
        {
          "type": "command",
          "command": "mcp.invoke",
          "args": {
            "provider": "github-mcp",
            "command": "list_issues"
          }
        }
      ]
    },
    {
      "name": "GitHub - Issues - Comentar",
      "description": "Adicionar comentário a issue via MCP",
      "pattern": "/comentar( na)? issue|comment( on)? issue/i",
      "actions": [
        {
          "type": "command",
          "command": "mcp.invoke",
          "args": {
            "provider": "github-mcp",
            "command": "add_issue_comment"
          }
        }
      ]
    },
    {
      "name": "GitHub - Pull Request - Criar",
      "description": "Criar PR via MCP",
      "pattern": "/criar pr|novo pr|new pr|create pull request/i",
      "actions": [
        {
          "type": "command",
          "command": "mcp.invoke",
          "args": {
            "provider": "github-mcp",
            "command": "create_pull_request"
          }
        }
      ]
    },
    {
      "name": "GitHub - Pull Request - Listar",
      "description": "Listar PRs via MCP",
      "pattern": "/listar pr|list pr|listar pull requests/i",
      "actions": [
        {
          "type": "command",
          "command": "mcp.invoke",
          "args": {
            "provider": "github-mcp",
            "command": "list_pull_requests"
          }
        }
      ]
    },
    {
      "name": "GitHub - Pull Request - Merge",
      "description": "Fazer merge de PR via MCP",
      "pattern": "/merge pr|merge pull request/i",
      "actions": [
        {
          "type": "command",
          "command": "mcp.invoke",
          "args": {
            "provider": "github-mcp",
            "command": "merge_pull_request"
          }
        }
      ]
    },
    {
      "name": "GitHub - Review - Aprovar PR",
      "description": "Aprovar PR via MCP",
      "pattern": "/aprovar pr|approve pr|approve pull request/i",
      "actions": [
        {
          "type": "function",
          "function": "approveCurrentPR"
        }
      ]
    },
    {
      "name": "GitHub - Review - Comentar PR",
      "description": "Comentar em PR via MCP",
      "pattern": "/comentar( no)? pr|comment( on)? pr/i",
      "actions": [
        {
          "type": "command",
          "command": "mcp.invoke",
          "args": {
            "provider": "github-mcp",
            "command": "add_pull_request_review_comment"
          }
        }
      ]
    },
    {
      "name": "GitHub - Search - Código",
      "description": "Buscar código via MCP",
      "pattern": "/buscar código|search code/i",
      "actions": [
        {
          "type": "command",
          "command": "mcp.invoke",
          "args": {
            "provider": "github-mcp",
            "command": "search_code"
          }
        }
      ]
    },
    {
      "name": "GitHub - Search - Repositórios",
      "description": "Buscar repositórios via MCP",
      "pattern": "/buscar repos|search repos/i",
      "actions": [
        {
          "type": "command",
          "command": "mcp.invoke",
          "args": {
            "provider": "github-mcp",
            "command": "search_repositories"
          }
        }
      ]
    },
    {
      "name": "GitHub - Search - Issues",
      "description": "Buscar issues via MCP",
      "pattern": "/buscar issues|search issues/i",
      "actions": [
        {
          "type": "command",
          "command": "mcp.invoke",
          "args": {
            "provider": "github-mcp",
            "command": "search_issues"
          }
        }
      ]
    }
  ],
  "functions": {
    "approveCurrentPR": {
      "code": "async function approveCurrentPR() {\n  // Este é um exemplo de como você poderia implementar uma função personalizada\n  // para aprovar o PR atual no qual você está trabalhando\n  \n  // 1. Obter informações do repositório atual\n  const currentRepo = await mcp.invoke({\n    provider: 'github-mcp',\n    command: 'get_me'\n  });\n  \n  // 2. Obter o PR atual (simplificado - na prática você precisaria determinar qual PR)\n  const pr = await mcp.invoke({\n    provider: 'github-mcp',\n    command: 'get_pull_request',\n    args: {\n      owner: 'OWNER',  // Substitua com valores dinâmicos\n      repo: 'REPO',     // em uma implementação real\n      pullNumber: 1\n    }\n  });\n  \n  // 3. Aprovar o PR\n  return await mcp.invoke({\n    provider: 'github-mcp',\n    command: 'create_pull_request_review',\n    args: {\n      owner: 'OWNER',  // Substitua com valores dinâmicos\n      repo: 'REPO',     // em uma implementação real\n      pullNumber: 1,\n      event: 'APPROVE',\n      body: 'Looks good to me! Approved via MCP.'\n    }\n  });\n}"
    }
  }
}
```

Este arquivo de regras do Cursor engloba a maioria dos comandos MCP GitHub e fornece atalhos para as operações mais comuns. Você pode personalizar qualquer uma dessas regras para se adequar ao seu fluxo de trabalho específico.

### Como usar estas regras:

1. Salve a configuração acima em um arquivo `.cursor/rules.json` no seu projeto
2. Ative as regras nas configurações do Cursor
3. Use os padrões de texto correspondentes em suas conversas com o assistente do Cursor

Por exemplo, digitar "criar pr" irá automaticamente iniciar o fluxo para criar um pull request usando o MCP GitHub, sem precisar digitar comandos Git ou usar a linha de comando.

---

Este documento serve como uma referência completa para todos os comandos MCP GitHub disponíveis no Cursor. Consulte a [documentação oficial do Cursor sobre MCP](https://cursor.sh/docs/mcp) para informações mais atualizadas. 