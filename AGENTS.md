# AGENTS — Instruções ao Codex

Objetivo: implementar incrementalmente o Multi-Agent Workspace conforme `SPEC.md`, `ARCHITECTURE.md`, `SPIKE.md` e `DECISIONS.md`.

## Ordens de execução
1. Ler estes arquivos antes de alterar código. Em conflito, reportar ambiguidade e preservar decisões explícitas.
2. Começar somente pelos Spikes A e B. Não antecipar VS Code nem delegação D enquanto os gates não forem aprovados.
3. Confirmar imports e assinaturas no código/documentação do PiDurable **da versão fixada**; não inventar APIs.
4. Fixar versões e lockfile, manter TypeScript estrito, criar testes unitários para schema e registry.
5. Toda chamada ao SDK deve ficar no módulo `pidurable-host`, sem interface genérica para outros harnesses.
6. Implementar segurança na execução de ferramentas, nunca apenas no prompt. Não gravar tokens, segredos ou prompts confidenciais em logs.
7. Toda entrega incluir: o que mudou, como testar, resultado dos testes, riscos, dúvidas e atualização do `HANDOFF.md`.
8. Não adicionar dependência de Codex CLI, Claude Code CLI, Gemini CLI, adaptadores de terminal, scraping, Gateway remoto ou orquestrador autônomo.

## Definição de pronto
Código reproduzível, testes documentados, critérios do spike correspondentes satisfeitos e limitações declaradas sem exageros.

## Primeiro prompt sugerido ao Codex Cloud
Leia SPEC.md, ARCHITECTURE.md, SPIKE.md, AGENTS.md e DECISIONS.md. Valide as APIs reais e versões compatíveis do PiDurable. Implemente apenas os Spikes A e B, com TypeScript estrito e testes do registro de agentes declarativos. Não implemente a extensão VS Code ainda. Atualize HANDOFF.md com comandos e resultados observados.
