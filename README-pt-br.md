# mcp-github-cursor-windows
Repositório de testes e desenvolvimento para integração do Cursor IDE com o GitHub MCP Server no Windows.

## Versão em Português Brasileiro (Padrão)

Este projeto demonstra como integrar o GitHub MCP Server ao Cursor IDE no Windows.

- [Instruções de configuração](#passo-a-passo)
- [Como funciona o MCP](#como-funciona-o-mcp)
- [Uso: Git x MCP](#uso-git-x-mcp)
- [Comandos em linguagem natural](#comandos-em-linguagem-natural)
- [Problemas comuns](#solução-de-problemas-comuns)
- [Regras Cursor para MCP](#regras-cursor-para-mcp)

## [English version](README.md)

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