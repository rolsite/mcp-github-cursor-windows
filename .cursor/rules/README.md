# Cursor Rules para MCP GitHub

Este diretório contém as regras Cursor para facilitar o uso do MCP GitHub.

## Regras disponíveis

### mcp-sync.mdc
- **Descrição**: Implementa sincronização automática entre MCP e Git Local
- **Tipo**: Always Apply (sempre aplicada)
- **Uso**: Sincroniza automaticamente o Git local após operações MCP

### mcp-commands.mdc
- **Descrição**: Define comportamentos para comandos MCP GitHub comuns
- **Tipo**: Auto Attached (aplicado em arquivos .md, .js, .ts)
- **Uso**: Fornece assistência com comandos MCP para arquivos e PRs

### mcp-repository.mdc
- **Descrição**: Operações de repositório via MCP GitHub
- **Tipo**: Auto Attached (aplicado em arquivos .md)
- **Uso**: Ajuda com criação, fork e outras operações de repositório

## Como usar

Estas regras são aplicadas automaticamente quando você interage com o Cursor AI. Elas ajudam a:

1. Manter o Git local sincronizado com operações MCP
2. Fornecer comandos MCP apropriados quando solicitado
3. Manter consistência nas operações do GitHub

## Adicionando novas regras

Para adicionar uma nova regra:
1. Crie um arquivo .mdc neste diretório
2. Adicione os metadados apropriados no topo do arquivo
3. Escreva o conteúdo da regra em markdown

Exemplo:
```
---
description: Minha nova regra
globs: ["**.ts"]
alwaysApply: false
---

# Título da regra

Conteúdo da regra... 