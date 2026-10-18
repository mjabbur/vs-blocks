# Multi-Agent Workspace — SPEC v0.1

Status: arquitetura aprovada para spike; implementação ainda não iniciada.

## Objetivo
Plataforma local de agentes especializados, executados por um único harness (`@earendil-works/pi-durable`), com interface no VS Code. Um agente novo deve ser adicionável por YAML e instruções Markdown, sem alterar o núcleo, desde que use modelos e ferramentas já registrados.

## Decisões vinculantes
- TypeScript e Node.js; runtime em processo separado do Extension Host.
- PiDurable como único harness; **não** integrar Codex CLI, Claude Code CLI ou Gemini CLI.
- Agentes definidos em YAML + arquivo de instruções Markdown.
- Delegação explícita e rastreável; **sem orquestrador autônomo** na v0.1.
- VS Code como interface inicial; aproveitar editores, diffs e visualização Markdown nativos.
- SQLite via PiDurable para persistência de conversas, sujeito à prova de recuperação no spike.
- Execução local; sem Gateway remoto ou protocolo de integração com CLIs de terceiros.

## Requisitos funcionais
1. Descobrir e validar arquivos `agents/*/agent.yaml` e suas instruções.
2. Resolver por agente: provedor/modelo, instruções, ferramentas permitidas, diretório de trabalho e parâmetros de execução.
3. Criar/reabrir conversas independentes e apresentar atualizações de execução.
4. Permitir seleção manual de agente e delegação explícita `Coding → Audit`, com identificação de origem, destino e resultado.
5. Disponibilizar no VS Code: escolha do agente, entrada de mensagens, histórico, streaming e links para arquivos/diffs.
6. Registrar falhas, decisões de permissão e execução de ferramentas com rastreabilidade suficiente para auditoria.
7. Retomar conversas após reinício sem duplicar submissões.

## Requisitos não funcionais
- Isolamento das chamadas de SDK em módulo pequeno `pidurable-host` (não adapter genérico de harness).
- Protocolo IPC local versionado com `requestId`, `sessionId`, `agentId`, evento/tipo, sequência quando aplicável e erro estruturado. Transporte final será escolhido no spike entre IPC do sistema/localhost seguro.
- O runtime é o único proprietário das instâncias de PiDurable; a extensão não abre o mesmo banco diretamente.
- Segredos fora do YAML e dos logs; credenciais em ambiente/armazenamento seguro apropriado.
- Ferramentas destrutivas ou shell requerem autorização verificável na fronteira de execução; prompts não são mecanismo de segurança.
- Dependências fixadas por lockfile; testes automatizados.

## Fora de escopo da v0.1
Orquestrador autônomo, múltiplos harnesses, integração com CLIs, conexão remota, sandbox robusta multiusuário, marketplace de plugins, cobrança unificada e interface Android.

## Critérios de sucesso
Spikes A–D em `SPIKE.md`; especialmente criar quarto agente por configuração, sem mudança no núcleo, executar delegação explícita e recuperar conversa após reinício.

## Riscos e hipóteses
- API do PiDurable é experimental: confirmar comportamento na versão fixada.
- Autenticação e cobrança de APIs de modelos **não** são garantidas pelas assinaturas dos CLIs.
- Isolamento por seleção de ferramentas não equivale a isolamento no sistema operacional.
- Capacidade de eventos em tempo real, retomada e assinaturas IPC exigem prova de ponta a ponta.

## Referências técnicas
- https://github.com/earendil-works/pi/blob/main/packages/durable/README.md
