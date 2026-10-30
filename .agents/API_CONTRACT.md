# Contrato do Assistant Bridge (proposta)

> Contrato de **alto nível**, ainda não implementado. A issue de compatibilidade verifica OpenCode v2 e fixa rotas/dados. Não acoplar Flutter diretamente às rotas internas do OpenCode.

## Princípios
- API versionada em `/api/v1`, JSON com schemas explícitos, timestamps UTC ISO 8601, erros com código estável, `request_id` e mensagem legível.
- Sem shell genérico no Bridge. O único campo de texto livre entra como **mensagem do usuário** em sessão do OpenCode, e a ferramenta do agente fica sujeita às políticas do OpenCode.
- Eventos via SSE ou WebSocket com `event_id`, `session_id`, sequência e suporte a reconexão; repetir evento não pode repetir execução.
- Segredos de providers/OpenCode ficam no PC. Cliente recebe somente metadados necessários.
- Pareamento exige evento presencial no desktop; todas as demais rotas exigem credencial por dispositivo e checagem de escopo.
- Operações sensíveis exigem `approval_id`, identificação exata do pedido, expiração curta e estado idempotente.

## Recursos candidatos

| Método | Rota | Finalidade |
|---|---|---|
| GET | `/api/v1/health` | versão e status básico, sem vazar dados |
| GET | `/api/v1/capabilities` | versões, modos, voz, permissões disponíveis |
| POST | `/api/v1/pairing/start` | apenas local: abrir pareamento temporário |
| POST | `/api/v1/pairing/complete` | finalizar chave de dispositivo após aceite |
| GET | `/api/v1/devices` | dispositivos ativos, escopos e último acesso |
| DELETE | `/api/v1/devices/{id}` | revogar dispositivo, somente autoridade local |
| GET | `/api/v1/sessions` | listar sessões visíveis por escopo |
| POST | `/api/v1/sessions` | iniciar sessão pessoal ou de projeto |
| GET | `/api/v1/sessions/{id}/messages` | recuperar histórico |
| POST | `/api/v1/sessions/{id}/messages` | enviar mensagem com `idempotency_key` |
| GET | `/api/v1/events` | stream global de sessões e aprovações |
| POST | `/api/v1/sessions/{id}/cancel` | cancelar tarefa |
| GET | `/api/v1/providers` | listar agentes/modelos disponíveis sem chaves |
| GET | `/api/v1/approvals` | solicitações pendentes |
| POST | `/api/v1/approvals/{id}/decision` | aprovar/negar ação exata |
| GET | `/api/v1/actions` | catálogo de operações administrativas permitidas |
| POST | `/api/v1/actions/{id}/request` | solicitar operação tipada ao broker |

## Estados de sessão
`created` → `running` → `completed` ou `failed`/`cancelled`. `waiting_approval` é estado transitório. Operações de alteração exigem confirmação antes da execução, não confirmação retroativa.

## Eventos mínimos
`session.created`, `message.delta`, `message.completed`, `tool.started`, `tool.completed`, `approval.requested`, `approval.resolved`, `session.state`, `connection.error`.

## Limites
- Rate limiting por dispositivo, tamanho máximo de payload, timeout e backpressure no streaming.
- Nunca armazenar segredo/senha em payload, log ou notificação.
- Cancelamento e retries sem duplicação de ações.
- Dispositivos Android podem iniciar com escopo `chat:read` e `chat:write`; mudanças e aprovações ganham escopos separados, configuráveis.

## OpenCode
Camada `OpenCodeAdapter` transforma API/SDK oficial em `session.create/list`, `prompt.send`, `events.subscribe`, `permissions.respond`, `session.cancel`. Antes de implementação, verificar se a versão atual dispõe desses métodos e registrar divergências. Não assumir que SDK v1 e v2 são compatíveis.
