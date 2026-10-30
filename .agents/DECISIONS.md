# Registro de decisões (ADRs leves)

## D-001 — Flutter como UI comum
**Status:** proposta aceita para planejamento. Linux e Android compartilham interface, regras e traduções. Extensões GNOME/Android por adaptadores. Motivo: preferência do projeto por Flutter e baixo retrabalho. Revisar overhead de startup no MVP.

## D-002 — OpenCode como motor, não como dependência embutida no celular
**Status:** proposta. O PC concentra modelos, providers, ferramentas, MCP e acesso aos arquivos. Android é cliente remoto; não executa OpenCode localmente.

## D-003 — Bridge TypeScript isolado
**Status:** proposta. Serviço Node/TypeScript usa SDK compatível do OpenCode com adaptador e API pequena para Flutter. Trade-off: processo Node adicional, compensado pela integração direta com SDK JS/TS. Avaliar API oficial v2 antes de fixar a implementação.

## D-004 — Segurança de acesso remoto
**Status:** proposta. Bridge e OpenCode em loopback, acesso Android via Tailscale Serve e HTTPS privado, mais credencial por dispositivo. Sem portas públicas, Funnel ou webhooks que executem comandos.

## D-005 — Nenhuma senha sudo armazenada
**Status:** decisão de segurança. Não armazenar senha de sudo em Keychain, Android Keystore, arquivo, configuração, Bridge ou contexto do LLM. Para tarefas administrativas: broker polkit de operações cadastradas, com autorização granular e auditoria.

## D-006 — Permissões por risco
**Status:** proposta. Chat/leitura, edição, ações destrutivas e operações root possuem escopos e confirmações separados. O Android não recebe root implícito por estar pareado.

## D-007 — GNOME/Wayland primeiro
**Status:** proposta. Primeira distribuição para Fedora GNOME, com integração nativa onde suportada; evitar prometer always-on-top e atalhos globais universais no Wayland. Futuro Linux em outras distros.

## D-008 — Voz gradual
**Status:** proposta. Push-to-talk primeiro; resposta falada opcional. Wake word e conversação contínua só depois do MVP e sempre opt-in.

## D-009 — Sem sincronização de dados própria desnecessária
**Status:** proposta. OpenCode é a fonte de verdade para mensagens/sessões; Bridge guarda apenas pareamento, políticas e audit trail. Não duplicar a base interna do OpenCode.

## D-010 — Idiomas e aparência
**Status:** proposta. pt-BR padrão; en/es disponíveis; aparência claro/escuro automática.
