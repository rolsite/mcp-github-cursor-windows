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