# mcp-github-cursor-windows
A GitHub repository for testing and development with Cursor IDE integration

# Tutorial: Configurando o GitHub MCP Server no Cursor

## O que é o MCP Server?

O Model Context Protocol (MCP) é um protocolo para estender as capacidades do Cursor permitindo integração com ferramentas externas. O github-mcp-server é a implementação oficial do GitHub para este protocolo.

## Pré-requisitos

- Go instalado no Windows
- Git instalado
- Cursor IDE instalado
- Token de acesso pessoal do GitHub

## Passo a Passo

### 1. Instalar o github-mcp-server

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

Salve este script como `install_mcp_server.bat` e execute-o em um Prompt de Comando.

### 2. Criar um token de acesso pessoal do GitHub

1. Acesse https://github.com/settings/tokens
2. Clique em "Generate new token" (Classic)
3. Dê um nome ao token (ex: "Cursor MCP Integration")
4. Selecione os escopos necessários (pelo menos "repo" e "read:user")
5. Gere o token e copie-o para uso posterior

### 3. Configurar o MCP no Cursor (Configuração Global)

1. Localize a pasta de configuração global do Cursor:
   - Windows: `C:\Users\[SeuUsuário]\.cursor`
   - No seu caso: `C:\Users\Usuario\.cursor`

2. Crie um arquivo chamado `mcp.json` dentro desta pasta (se não existir)

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
2. Nas configurações do Cursor, na seção MCP, o servidor GitHub deve aparecer na lista de ferramentas disponíveis

## Solução de Problemas Comuns

### Erro: "mcpServers must be an object"

Verifique se a estrutura do seu arquivo JSON está correta. O campo `mcpServers` deve ser um objeto (entre chaves `{}`), não um array.

### Erro: "JSON syntax error: Unexpected end of JSON input"

Certifique-se de que seu arquivo JSON está completo e corretamente formatado, com todas as chaves e colchetes fechados.

### Erro: "failed to create client"

1. Verifique se o caminho para o executável está correto
2. Certifique-se de ter incluído o argumento `stdio` 
3. Confirme se o token do GitHub é válido

## Usando o GitHub MCP Server

Agora você pode usar os comandos do GitHub diretamente no Cursor, como:

- Criar pull requests
- Visualizar arquivos de repositórios
- Fazer fork de repositórios
- Gerenciar branches
- E muito mais!

---

Com esta configuração, o Cursor pode interagir diretamente com o GitHub, permitindo uma experiência de desenvolvimento mais integrada e eficiente.
