# 🚀 Integração MCP GitHub para Cursor IDE (Windows)

<div align="center">
  
![Integração MCP GitHub](images/mcp-cursor-demo.png)

**Integre operações do GitHub perfeitamente no Cursor IDE no Windows**

[Instalação](#-instalação) • 
[Como Funciona](#-como-funciona) • 
[Guia de Uso](#-guia-de-uso) • 
[Comandos](#-comandos-em-linguagem-natural) • 
[Solução de Problemas](#-solução-de-problemas) • 
[Recursos](#-recursos)

*[English Version](README.md)*

</div>

---

## 📋 Visão Geral

Este projeto demonstra como integrar o GitHub MCP (Model Context Protocol) Server com o Cursor IDE no Windows, permitindo realizar operações avançadas do GitHub diretamente do seu editor usando comandos em linguagem natural.

**Principais Benefícios:**
- Crie e gerencie issues e pull requests sem sair do seu IDE
- Revise, comente e aprove PRs diretamente no seu fluxo de trabalho
- Gerencie recursos do repositório GitHub de forma contínua
- Automatize fluxos de trabalho do GitHub Actions com comandos simples

---

## 🔧 Instalação

### Pré-requisitos

- [Go](https://go.dev/dl/) instalado no Windows
- [Git](https://git-scm.com/download/win) instalado
- [Cursor IDE](https://cursor.sh/) instalado
- [Token de acesso pessoal do GitHub](https://github.com/settings/tokens)

### Passo a Passo

<details>
<summary><b>1. Instalar o github-mcp-server</b></summary>

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
</details>

<details>
<summary><b>2. Criar um token de acesso pessoal do GitHub</b></summary>

1. Acesse https://github.com/settings/tokens
2. Clique em "Generate new token" (Classic)
3. Dê um nome ao token (ex: "Cursor MCP Integration")
4. Selecione os escopos necessários (pelo menos "repo" e "read:user")
5. Gere o token e copie-o para uso posterior
</details>

<details>
<summary><b>3. Configurar o MCP no Cursor</b></summary>

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
</details>

<details>
<summary><b>4. Verificar a configuração</b></summary>

1. No Cursor, você deverá ver uma mensagem de confirmação que o MCP está configurado
2. Nas configurações do Cursor (Configurações > MCP), o servidor GitHub deve aparecer na lista de ferramentas disponíveis
</details>

---

## 🔄 Como Funciona

O MCP (Model Context Protocol) permite que seu IDE se comunique diretamente com o GitHub de uma maneira que vai além das operações Git padrão:

- **Integração Direta com API:** O MCP se comunica com a API do GitHub para realizar ações que normalmente exigiriam a interface web
- **Processamento de Linguagem Natural:** Os comandos são interpretados e executados com base em instruções em linguagem comum
- **Reflexo Imediato no Repositório Remoto:** Todas as operações são executadas diretamente no repositório remoto em tempo real
- **Complementar ao Git:** O controle de versão padrão ainda usa o Git local para desempenho ideal

---

## 📘 Guia de Uso

### Fluxo Local Git (Operações Padrão)

Use o Git padrão para todas as operações de controle de versão local:

| Operação | Comando Git | Descrição |
|-----------|------------|-------------|
| Commits | `git commit -m "mensagem"` | Registrar alterações no repositório |
| Branches | `git branch`, `git checkout` | Criar e alternar entre branches |
| Histórico | `git log` | Visualizar histórico de commits |
| Mesclagem | `git merge`, `git rebase` | Combinar alterações de branches |
| Sincronização | `git push`, `git pull` | Sincronizar com repositório remoto |

> Estes comandos são executados no terminal ou pelo painel de Controle de Código no Cursor IDE.

### Fluxo MCP (Recursos Avançados do GitHub)

Use o MCP para operações específicas do GitHub através de linguagem natural:

| Operação | Exemplo de Comando MCP | Equivalente no GitHub |
|-----------|---------------------|-------------------|
| Issues | "Crie uma nova issue com título 'Bug na página de login'" | Criar issue pela UI do GitHub |
| Pull Requests | "Abra um pull request de feature/login para main" | Abrir PR no site do GitHub |
| Revisões | "Aprove o pull request #42" | Revisar PR no site do GitHub |
| Labels | "Adicione o label 'urgente' à issue #10" | Gerenciar labels pela UI do GitHub |
| Repositório | "Forke este repositório" | Operações de repositório no GitHub |

> Estes comandos são escritos em linguagem natural diretamente no Cursor IDE.

---

## 💬 Comandos em Linguagem Natural

O MCP aceita comandos em português e inglês. Aqui estão alguns exemplos:

### Comandos em Português

- "Crie uma nova issue com o título 'Bug na tela de login'"
- "Liste todas as issues abertas"
- "Feche a issue #12"
- "Abra um pull request de feature/login para main"
- "Liste todos os pull requests abertos"
- "Faça merge do pull request #42"
- "Aprove o pull request #42"
- "Adicione um comentário ao pull request #42: Está ótimo!"
- "Adicione o label 'urgente' à issue #10"
- "Atribua a issue #10 para @usuario"

Para referência completa de comandos, veja `.cursor/rules/mcp-commands-ptbr.mdc`.

### Comandos em Inglês

Veja `.cursor/rules/mcp-commands.mdc` para comandos em inglês.

---

## ❓ Solução de Problemas

| Problema | Solução |
|---------|----------|
| **Arquivos locais desatualizados** | Execute `git fetch` e `git reset --hard origin/main` após operações do MCP |
| **Erros de token** | Verifique a validade do token e os escopos nas configurações do GitHub |
| **MCP server não encontrado** | Verifique os caminhos na configuração e a instalação do servidor |
| **Comando não reconhecido** | Consulte a referência de comandos e garanta a fraseologia adequada |

---

## 📚 Recursos

- **Arquivos de Configuração:**
  - `.cursor/rules/mcp-commands.mdc`: Exemplos de comandos em inglês
  - `.cursor/rules/mcp-commands-ptbr.mdc`: Exemplos de comandos em português

- **Recursos Externos:**
  - [Documentação da API do GitHub](https://docs.github.com/pt/rest)
  - [Documentação do Cursor IDE](https://cursor.sh/docs)

---

<div align="center">
  
*Este README é a versão em português brasileiro. Para instruções em inglês, veja [README.md](README.md).*

</div> 