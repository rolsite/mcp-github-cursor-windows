# mcp-github-cursor-windows
Repositório de testes e desenvolvimento para integração do Cursor IDE com o GitHub MCP Server no Windows.

## Versão em Português Brasileiro (Padrão)

Este projeto demonstra como integrar o GitHub MCP Server ao Cursor IDE no Windows.

- [Instruções de instalação](#instalação)
- [Como funciona o MCP](#como-funciona-o-mcp)
- [Uso: Git x MCP](#uso-git-x-mcp)
- [Comandos em linguagem natural](#comandos-em-linguagem-natural)
- [Problemas comuns](#solução-de-problemas-comuns)
- [Regras Cursor para MCP](#regras-cursor-para-mcp)

## [English version](README.md)

---

## Instalação

### Pré-requisitos

- [Go](https://go.dev/dl/) instalado no Windows
- [Git](https://git-scm.com/download/win) instalado
- [Cursor IDE](https://cursor.sh/) instalado
- [Token de acesso pessoal do GitHub](https://github.com/settings/tokens)

### Passo a Passo

#### 1. Instalar o github-mcp-server

Crie um arquivo chamado `install_mcp_server.bat` com o seguinte conteúdo:

```batch
@echo off
set REPO_URL=https://github.com/github/github-mcp-server.git
echo Criando diretório de instalação...
mkdir C:\MCP\github
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
```

Execute este script como administrador no Prompt de Comando.

#### 2. Criar um token de acesso pessoal do GitHub

1. Acesse https://github.com/settings/tokens
2. Clique em "Generate new token" (Classic)
3. Dê um nome ao token (ex: "Cursor MCP Integration")
4. Selecione os escopos necessários (pelo menos "repo" e "read:user")
5. Gere o token e copie-o para uso posterior

#### 3. Configurar o MCP no Cursor (Configuração Global)

1. Localize a pasta de configuração global do Cursor:
   - Windows: `C:\Users\[SeuUsuario]\.cursor`
2. Crie um arquivo chamado `mcp.json` nesta pasta (se não existir)
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

> **Nota:** Esta configuração global torna o MCP server disponível em todos os projetos. Alternativamente, você pode criar uma configuração específica por projeto colocando o arquivo `mcp.json` na pasta `.cursor` dentro do diretório do projeto.

#### 4. Verificar a configuração

1. No Cursor, você deverá ver uma mensagem de confirmação que o MCP está configurado
2. Nas configurações do Cursor (Configurações > MCP), o servidor GitHub deve aparecer na lista de ferramentas disponíveis

## MCP Server no Cursor IDE

Abaixo, um exemplo do MCP Server configurado no Cursor IDE:

![MCP Server funcionando no Cursor](images/mcp-cursor-demo.png)

---

## Como funciona o MCP

- O MCP (Model Context Protocol) permite que o Cursor interaja diretamente com o GitHub, indo além dos comandos Git locais para recursos avançados do GitHub.
- Todas as operações do MCP são executadas via comandos em linguagem natural e refletem imediatamente no repositório remoto.
- O Git local continua sendo usado para todas as operações normais de versionamento.

## Uso: Git x MCP

### O que o Git local faz (fluxo padrão)
Use o Git integrado para todas as operações normais de versionamento:
- Commit local (`git commit`)
- Criação e troca de branches (`git branch`, `git checkout`)
- Merge, rebase, cherry-pick (`git merge`, `git rebase`, `git cherry-pick`)
- Push e pull (`git push`, `git pull`)
- Resolução de conflitos
- Histórico e log (`git log`)
- Stash, reset, checkout, etc.

**Exemplo de comando:**
- `git commit -m "minha mensagem"`
- `git push`
- `git checkout feature/login`

> Esses comandos são executados no terminal ou pelo painel de controle de código-fonte do editor.

### O que o MCP faz (integração avançada com o GitHub)
Use o MCP para tarefas que o Git local não cobre, como:
- Criar, listar ou gerenciar issues do GitHub diretamente pelo editor
- Criar, listar ou gerenciar pull requests (PRs) diretamente pelo editor
- Comentar, revisar e aprovar PRs sem sair do ambiente
- Gerenciar labels, milestones e responsáveis de issues/PRs
- Visualizar e interagir com alertas de segurança, code scanning e secret scanning
- Operações administrativas do repositório (forkar, criar repositório, configurar integrações)
- Automatizar fluxos de trabalho do GitHub Actions

**Exemplos de comandos em linguagem natural:**
- "Crie uma nova issue com o título 'Bug na tela de login'"
- "Liste todos os pull requests abertos"
- "Aprove o pull request #42"
- "Adicione o label 'urgente' à issue #10"
- "Forke este repositório"

> Esses comandos são escritos em linguagem natural e executados pela integração MCP no Cursor. A operação correspondente da API do GitHub é acionada automaticamente.

## Comandos em linguagem natural

- O MCP permite usar linguagem natural para operações avançadas do GitHub. Veja exemplos em `.cursor/rules/mcp-commands-ptbr.mdc` (português) e `.cursor/rules/mcp-commands.mdc` (inglês).
- Para versionamento padrão, continue usando os comandos Git normalmente.

## Solução de Problemas Comuns

- **Arquivos locais desatualizados:** Sempre execute `git fetch` e `git reset --hard origin/main` após operações MCP que alterem o repositório remoto.
- **Erros de token:** Certifique-se de que seu token do GitHub é válido e possui os escopos corretos.
- **MCP server não encontrado:** Verifique os caminhos de configuração e se o servidor está compilado.

## Regras Cursor para MCP

- Veja `.cursor/rules/mcp-integration.mdc` para saber quando usar Git ou MCP.
- Veja `.cursor/rules/mcp-commands-ptbr.mdc` e `.cursor/rules/mcp-commands.mdc` para exemplos de comandos em linguagem natural.

---

Este README é a versão em português brasileiro. Para a versão em inglês, veja [README.md](README.md). 