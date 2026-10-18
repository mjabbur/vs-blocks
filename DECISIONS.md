# DECISIONS — Registro arquitetural v0.1

## D-001 — Um único harness: PiDurable (aprovada)
**Motivo:** evitar raspagem de tela, parsing de saídas e integração quebradiça com CLIs comerciais atualizados frequentemente.
**Consequência:** a compatibilidade com modelos será via APIs de provedores; assinaturas de aplicativos não são equivalentes a créditos de API. SDK experimental exige versões fixadas.

## D-002 — Node.js + TypeScript (aprovada)
**Motivo:** stack única para gerência, SDK e extensão VS Code; menos pontos de tradução.

## D-003 — Runtime separado do VS Code (aprovada)
**Motivo:** separar ciclo de vida, processo e posse das sessões persistentes; IPC controlado. Não acessar SQLite do mesmo harness por dois processos gestores.

## D-004 — YAML + Markdown (aprovada)
**Motivo:** criação de agente sem alterar núcleo; ferramentas novas podem demandar plugin de código.

## D-005 — Delegação explícita mínima (aprovada)
**Motivo:** primeiro validar previsibilidade, isolamento e auditoria; orquestrador autônomo é evolução compatível, não escopo v0.1.

## D-006 — VS Code como interface (aprovada)
**Motivo:** reutilizar editores, diffs, Markdown e affordances existentes; webview apenas quando necessário.

## D-007 — GitHub como fonte de verdade após publicação (aprovada)
Consolidar decisões e testes nos arquivos versionados. Histórico de conversa serve de apoio, não substitui a documentação.

## D-008 — Escopo do primeiro ciclo (aprovada)
Spikes A e B primeiro; C e D depois. Não construir Gateway remoto, infraestrutura distribuída ou integração CLI.

## A validar
- APIs e versões reais do PiDurable; estrutura de observação e retomada.
- Transporte IPC local e experiência de reinício.
- Requisitos de proteção de ferramentas e workspace.
- Modelos/provedores disponíveis e autenticação/custos.
- Semântica exata das ferramentas e políticas no runtime.

## Referência
https://github.com/earendil-works/pi/blob/main/packages/durable/README.md
