# Arquitetura — OpenCode Assistant

## Objetivos
- Interface de conversa com abertura quase instantânea no Linux e cliente Android.
- Único motor de agentes: OpenCode rodando como usuário normal no Fedora.
- Acesso remoto ao PC sem shell remoto ou porta pública exposta.
- Sessões pessoais e de projeto, streaming, estado contínuo, pareamento e autorização.
- Base pragmática para manutenção e versões futuras, sem reimplementar OpenChamber.

## Componentes e fronteiras

| Camada | Tecnologia proposta | Responsabilidades |
|---|---|---|
| Cliente Linux | Flutter/Dart | overlay GNOME, entrada, chat, notificações, sessão, estado offline, configuração |
| Cliente Android | mesmo projeto Flutter | chat, voz, aprovações, notificações, pareamento, deep links |
| Assistant Bridge | Node.js/TypeScript, `systemd --user` | API autenticada, dispositivo, política de ações, adaptador OpenCode, streaming, logs |
| OpenCode | servidor local/SDK compatível fixado | agentes, modelos, ferramentas, MCP, sessões, permissões de execução |
| Transporte remoto | Tailscale Serve + HTTPS | exposição privada de Bridge apenas à tailnet do usuário |
| Broker privilegiado | serviço isolado de operações tipadas + polkit | ações administrativas restritas e auditáveis, sem receber prompts |

## Fluxo principal
1. Flutter autentica no Bridge (cliente Linux local também autentica, não confiar só no loopback).
2. Seleciona modo pessoal/projeto, sessão, agente/modelo conforme disponíveis.
3. Bridge aplica contexto e limites; solicita sessão ao OpenCode e encaminha mensagem.
4. Bridge reproduz eventos normalizados de tokens, ferramentas, pedidos de aprovação, erros e conclusão.
5. UI pode ocultar-se: backend e OpenCode continuam a tarefa; ao reabrir, reconcilia estado e histórico.
6. Quando ação exigir autorização, Bridge pausa/encaminha pedido, exige decisão vinculada à ação e expira solicitações.
7. Para operação administrativa, o agente **não recebe root**: solicita uma operação previamente cadastrada; Bridge exige política/consentimento e chama broker.

## Estratégia de processos
- Bridge e OpenCode executam sem root. Serviço de usuário reinicia em falha e não depende de janela visível.
- OpenCode escuta somente em `127.0.0.1` e usa autenticação de servidor quando aplicável.
- Bridge escuta em `127.0.0.1` (porta configurável) e usa autenticação por dispositivo. `tailscale serve` faz a ponte privada HTTPS para o Android.
- Evitar processo OpenCode duplicado se outro serviço do usuário já o administra; detectar instância e conectar ou iniciar a própria instância gerenciada com ciclo de vida claro.
- Não reutilizar arquivos internos não documentados do OpenChamber. Sua coexistência não deve quebrar sessões existentes.

## Versionamento
- Adaptador de OpenCode isolado por interface, sem SDK espalhado na UI.
- Primeira issue valida versão atual, API, streaming, permissões e reconexão; fixar versão e contrato.
- Bridge expõe API estável própria e sua versão semântica. Clientes negociam compatibilidade.
- Estados de sessão não devem ser persistidos em formato proprietário se o OpenCode já os mantém.

## Falhas previstas
- PC desligado/offline; sem Tailscale; sessão expirada; OpenCode não iniciado; upgrade incompatível; tarefa em andamento; negação de permissão.
- UI apresenta informação legível e ações de recuperação sem loops infinitos.
- Repetição de mensagem/ação deve ser idempotente; nunca duplicar execução perigosa por retry de rede.
- Streaming reconecta com cursor/event ID e recupera estado autoritativo no backend.

## Fora do MVP
- Automação genérica do desktop, controle por voz contínuo hands-free, execução privilegiada arbitrária, exposição pública, multiplataforma de host além do Linux e controle de diversos PCs.
