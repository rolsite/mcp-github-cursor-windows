# mcp-github-cursor-windows
A GitHub repository for testing and development with Cursor IDE integration

# Tutorial: Configurando o GitHub MCP Server no Cursor

![MCP Server funcionando no Cursor](https://raw.githubusercontent.com/rolsite/mcp-github-cursor-windows/main/images/mcp-cursor-demo.png)

## O que é o MCP Server?

O Model Context Protocol (MCP) é um protocolo para estender as capacidades do Cursor permitindo integração com ferramentas externas. O github-mcp-server é a implementação oficial do GitHub para este protocolo.

## Pré-requisitos

- [Go](https://go.dev/dl/) instalado no Windows
- [Git](https://git-scm.com/download/win) instalado
- [Cursor IDE](https://cursor.sh/) instalado
- [Token de acesso pessoal](https://github.com/settings/tokens) do GitHub

## Passo a Passo

### 1. Instalar o github-mcp-server

Primeiro, crie um arquivo chamado `install_mcp_server.bat` em qualquer local do seu computador (por exemplo, na Área de Trabalho) com o seguinte conteúdo:

```batch
@echo off
set REPO_URL=https://github.com/github/github-mcp-server.git

echo Verificando e criando diretório de instalação...
if not exist C:\MCP mkdir C:\MCP
if not exist C:\MCP\github mkdir C:\MCP\github
cd C:\MCP\github

echo Clonando o repositório do GitHub...
git clone %REPO_URL%
if errorlevel 1 (
    echo Erro ao clonar o repositório.
    exit /b 1
)

cd github-mcp-server
echo.
echo Compilando o MCP Server...
go build -o mcp-server.exe ./cmd/github-mcp-server
if errorlevel 1 (
    echo Erro ao compilar o MCP Server.
    exit /b 1
)

echo.
echo Copiando executável para o diretório final...
copy mcp-server.exe C:\MCP\github\
if errorlevel 1 (
    echo Erro ao copiar o executável.
    exit /b 1
)

echo.
echo Execução de teste do MCP Server (exibindo ajuda)...
C:\MCP\github\mcp-server.exe --help
if errorlevel 1 (
    echo Erro ao executar o MCP Server.
    exit /b 1
)

echo.
echo Instalação concluída com sucesso!
echo O executável foi instalado em C:\MCP\github\mcp-server.exe
pause
```

Em seguida, execute o script com permissões de administrador clicando com o botão direito no arquivo e selecionando "Executar como administrador".

> **Nota**: O script criará automaticamente os diretórios necessários (`C:\MCP\github`) se eles não existirem.

### 2. Criar um token de acesso pessoal do GitHub

1. Acesse https://github.com/settings/tokens
2. Clique em "Generate new token" (Classic)
3. Dê um nome ao token (ex: "Cursor MCP Integration")
4. Selecione os escopos necessários (pelo menos "repo" e "read:user")
5. Gere o token e copie-o para uso posterior

> **Importante**: Guarde esse token em um local seguro, pois ele não será mostrado novamente.

### 3. Configurar o MCP no Cursor (Configuração Global)

1. Localize a pasta de configuração global do Cursor:
   - Windows: `C:\Users\[SeuUsuario]\.cursor` (substitua [SeuUsuario] pelo seu nome de usuário do Windows)

2. Crie um arquivo chamado `mcp.json` dentro desta pasta (se não existir)
   - Se a pasta `.cursor` não existir, abra o Cursor pelo menos uma vez para que ela seja criada automaticamente

3. Adicione a seguinte configuração:

```json
{
  "mcpServers": {
    "github-mcp": {
      "command": "C:\\MCP\\github\\mcp-server.exe",
      "args": ["stdio"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "seu_token_aqui"
      }
    }
  }
}
```

4. Substitua `seu_token_aqui` pelo token que você gerou no GitHub
5. Salve o arquivo e reinicie o Cursor

> **Nota**: Esta configuração global torna o MCP server disponível em todos os projetos. Alternativamente, você pode criar uma configuração específica por projeto colocando o arquivo `mcp.json` na pasta `.cursor` dentro do diretório do projeto.

### 4. Verificar a configuração

1. No Cursor, você deverá ver uma mensagem de confirmação que o MCP está configurado
2. Nas configurações do Cursor (acesse em Settings > MCP), o servidor GitHub deve aparecer na lista de ferramentas disponíveis, como na imagem no início deste tutorial

### 5. Configurando Cursor Rules para priorizar o MCP

Para usar o MCP GitHub preferencialmente em vez dos comandos Git diretos, você pode configurar regras no Cursor. Crie um arquivo `rules.json` na pasta `.cursor` do seu projeto com o seguinte conteúdo:

```json
{
  "rules": [
    {
      "name": "GitHub - Commit",
      "description": "Usar MCP para commit",
      "pattern": "/git commit/i",
      "actions": [
        {
          "type": "suggest",
          "label": "Usar MCP GitHub",
          "message": "Recomendo usar o MCP GitHub para commits. Posso ajudar com isso?"
        }
      ]
    },
    {
      "name": "GitHub - Push",
      "description": "Usar MCP para push",
      "pattern": "/git push/i",
      "actions": [
        {
          "type": "suggest",
          "label": "Usar MCP GitHub",
          "message": "Você pode usar o MCP GitHub para fazer push. Quer que eu faça isso com MCP?"
        }
      ]
    },
    {
      "name": "GitHub - Pull Request",
      "description": "Criar PRs via MCP",
      "pattern": "/pull request|pr|merge/i",
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
    }
  ]
}
```

> **Importante**: Para evitar que este arquivo de configuração seja enviado ao seu repositório, adicione `.cursor/` ao seu arquivo `.gitignore`.

Exemplo de `.gitignore`:
```
# Ignorar pasta de configuração do Cursor
.cursor/
```

Com estas regras configuradas, o Cursor vai automaticamente:
- Sugerir o uso do MCP GitHub quando você tentar fazer commits ou pushes
- Iniciar automaticamente o fluxo de criação de PR via MCP quando você mencionar pull requests

Você pode personalizar as regras conforme suas necessidades e adicionar mais comandos MCP como fallbacks.

## Solução de Problemas Comuns

### Erro: "mcpServers must be an object"

Verifique se a estrutura do seu arquivo JSON está correta. O campo `mcpServers` deve ser um objeto (entre chaves `{}`), não um array.

### Erro: "JSON syntax error: Unexpected end of JSON input"

Certifique-se de que seu arquivo JSON está completo e corretamente formatado, com todas as chaves e colchetes fechados.

### Erro: "failed to create client"

1. Verifique se o caminho para o executável está correto
2. Certifique-se de ter incluído o argumento `stdio` 
3. Confirme se o token do GitHub é válido
4. Verifique se o Go está instalado corretamente e acessível no PATH

### Erro: "mcp-server.exe não é reconhecido como um comando"

Verifique se o caminho especificado no arquivo `mcp.json` está correto e se o executável foi corretamente gerado pelo script de instalação.

## Usando o GitHub MCP Server

Agora você pode usar os comandos do GitHub diretamente no Cursor, como:

- Criar pull requests
- Visualizar arquivos de repositórios
- Fazer fork de repositórios
- Gerenciar branches
- E muito mais!

Para uma lista completa de funcionalidades, consulte a [documentação oficial do Cursor sobre MCP](https://cursor.sh/docs/mcp).

---

Com esta configuração, o Cursor pode interagir diretamente com o GitHub, permitindo uma experiência de desenvolvimento mais integrada e eficiente.

## Nota de Atualização

Este documento foi atualizado diretamente usando o MCP GitHub, demonstrando a capacidade de fazer alterações em arquivos e enviá-las ao repositório sem precisar usar comandos Git tradicionais!

## Nota sobre MCP e Git Local

Para usar o MCP GitHub no Cursor e manter o Git local atualizado automaticamente, este projeto implementa sincronização automática usando Cursor Rules:

### Como funciona

1. **Sincronização Automática**: 
   - Quando qualquer comando MCP é detectado, o Git local é automaticamente sincronizado
   - Um comando `git fetch origin && git reset --hard origin/main` é executado após operações MCP
   - Você verá uma notificação confirmando a sincronização

2. **Comandos Específicos com Sincronização**:
   - Alguns comandos como `create_or_update_file` e `push_files` têm sincronização integrada
   - Após uma pequena espera para conclusão da operação, o Git local é sincronizado

3. **Sincronização Manual**:
   - Se necessário, digite "sincronizar git" ou "sync git" para forçar a sincronização

### Benefícios

- Use sempre o MCP para operações do GitHub
- O Cursor manterá o status do Git atualizado automaticamente
- Não é necessário lembrar de comandos Git adicionais

> **Nota**: A sincronização usa `git reset --hard`, que descarta qualquer mudança local não commitada. Certifique-se de ter feito commit de todas as mudanças importantes antes de usar operações MCP.
