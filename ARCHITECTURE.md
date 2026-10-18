# ARCHITECTURE — v0.1

## Visão
`VS Code Extension ⇄ IPC local ⇄ Node.js Agent Manager ⇄ PiDurable ⇄ APIs de modelos / ferramentas locais`

## Componentes
**Extensão VS Code**: UI de chat e seleção de agente; usa APIs nativas de editor, diff, Markdown, terminal e notificações. Nunca gerencia o estado do PiDurable diretamente.

**Runtime Node.js**: processo separado, iniciado e supervisionado pela extensão; recebe comandos de UI, envia eventos, controla ciclo de vida e encerra sem perder estado persistido.

**Agent Registry**: lê `agents/<id>/agent.yaml`, valida schema e referências de ferramentas/modelos, carrega `SYSTEM.md`, rejeita duplicatas e perfis inválidos. Mudanças em configuração passam a valer em uma nova sessão ou mediante operação explícita de reconfiguração; não devem alterar silenciosamente uma execução ativa.

**Agent Manager**: cria/reabre sessões, gerencia pedidos, executa delegações explícitas e consulta políticas. Não é um planejador autônomo.

**PiDurable Host**: módulo específico do SDK, responsável por criar registry interno, abrir armazenamento, configurar conversas, enviar inputs, observar resultados e retomar tarefas. Não implementar abstração multi-harness.

**Policy Gate**: checagem efetiva de acesso a ferramentas, paths e ações sensíveis antes da execução; o perfil `Audit` é somente leitura. Isolamento forte do SO é trabalho futuro.

## Contrato IPC conceitual (a verificar no spike)
Comandos: `agents.list`, `sessions.create`, `sessions.open`, `sessions.submit`, `sessions.cancel`, `delegations.request`, `permissions.respond`.
Eventos: `session.snapshot`, `run.started`, `run.delta`, `tool.requested`, `tool.completed`, `run.completed`, `run.failed`, `delegation.completed`.
Envelope mínimo: `{version, requestId, sessionId?, agentId?, type, payload}`; erros com código e mensagem. Não pressupor que nomes de eventos correspondam aos eventos internos do SDK.

## Persistência e ciclo de vida
- SQLite do PiDurable, local e controlado pelo runtime. Decidir localização e política de backups antes de produção.
- Runtime reabre storage e executa recuperação; verificar idempotência com `requestId`.
- Cliente após reconexão pede snapshot + estado atual; eventos efêmeros não são fonte da verdade.
- Trabalhos pendentes e falhas de retomada devem ser visíveis na interface.

## Schema declarativo preliminar
```yaml
id: audit
name: Audit
model:
  provider: openai
  id: MODEL_ID_FROM_ENV_OR_CONFIG
actions:
  tools: [read, git_diff]
instructions: SYSTEM.md
workspace: .
policy:
  readOnly: true
```
O schema acima é **nosso** e precisa ser validado/ajustado pelo spike; não é API nativa do PiDurable. Não registrar chaves de API no arquivo.

## Delegação explícita
Usuário ou fluxo pré-definido seleciona agente receptor, tarefa e artefatos; Manager cria pedido rastreável com IDs de origem/destino, submete na conversa do receptor e retorna resultado ao emissor/UI. Conversas e permissões são independentes; não transferir credenciais nem acesso por herança.

## Evolução futura
Orquestrador como componente acima do Agent Manager, consumindo os mesmos comandos e observabilidade, com políticas, orçamento, limites, confirmação humana e opção de desligar. **Não** implementar agora.
