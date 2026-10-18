# SPIKE — Plano de validação v0.1

Princípio: a documentação do SDK orienta os testes, mas não substitui execução real. Registrar versão do Node, versão de cada pacote, SO, comandos, logs redigidos, resultados e limitações.

## Preparação
1. Escolher e fixar versões de Node.js e dos pacotes PiDurable/pi-ai/chord; gerar lockfile.
2. Executar um exemplo mínimo com SQLite temporário e credenciais de API via ambiente; nunca commitar segredos.
3. Criar teste automatizado que não exija chamadas pagas para validar schema/registry e IPC.

## A — Runtime e retomada
- Abrir SQLite, criar conversa, enviar prompt e receber resposta/eventos.
- Executar ferramenta controlada e capturar resultado.
- Encerrar processo no meio de uma execução; reabrir e comprovar comportamento documentado.
- Reenviar `requestId` idêntico e verificar ausência de duplicação.
**Aceite:** evidência de persistência e sem execução duplicada; registrar limitações.

## B — Agentes declarativos
- Carregar `markdown`, `coding`, `audit` com instruções distintas e ferramentas adequadas.
- Rejeitar YAML inválido, ferramenta inexistente, referência fora do workspace e IDs repetidos.
- Adicionar quarto agente somente por YAML + Markdown; nenhum patch em `runtime/core`.
**Aceite:** agente aparece e executa com perfil correto; teste automático detecta alteração de configuração.

## C — VS Code
- Inicializar e desligar runtime separado.
- Listar agentes, trocar sessões, enviar mensagem e mostrar streaming; depois reabrir sessão.
- Abrir Markdown e diff nos editores nativos; mostrar erro e reconexão.
**Aceite:** fluxo operacional no VS Code sem terminal de CLI terceirizado.

## D — Delegação explícita
- Coding entrega alteração simples; usuário aciona Audit para revisão somente leitura.
- Correlacionar tarefa, resultados e IDs de ambas as conversas.
- Testar negativa de tentativa de escrita do Audit na camada de execução.
**Aceite:** isolamento de histórico e permissões, com trilha auditável.

## Gates
Iniciar implementação completa da extensão somente após A e B. Considerar orquestrador autônomo apenas depois de A–D e análise de segurança. Se SDK impedir um requisito, registrar bloqueio e rever SPEC antes de criar workaround frágil.

## Relatório esperado
Tabela por caso: ID, versão, ambiente, comando, esperado, observado, PASS/FAIL/BLOCKED, evidência e issue. Publicar em `HANDOFF.md` ou relatório de execução.
