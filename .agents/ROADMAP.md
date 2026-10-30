# Roadmap — OpenCode Assistant

> Ordem técnica proposta, sem datas fixas. Durações são **estimativas relativas**, não compromissos. Acompanhamento real pelas issues do repositório.

| Fase | Escopo | Dependências | Estimativa de esforço |
|---|---|---|---|
| F0 — Fundação | compatibilidade OpenCode/API, scaffold Flutter/Bridge, contrato e CI básico | nenhuma | 1–2 semanas |
| F1 — MVP Linux | serviço local, chat/streaming, overlay GNOME, modos pessoal/projeto, estados e permissões | F0 | 2–4 semanas |
| F2 — Android privado | pareamento, armazenamento seguro de token, Tailscale, sessão remota e reconexão | F1 e segurança | 2–3 semanas |
| F3 — Experiência | voz push-to-talk/TTS, notificações, anexos, sessões/dispositivos e configurações | F1–F2 | 2–3 semanas |
| F4 — Admin seguro | modelo de ações, broker polkit e operações explicitamente autorizadas | autenticação, políticas, auditoria | 2–4 semanas |
| F5 — Distribuição | pacote Fedora, APK, docs de instalação e hardening final | fases anteriores | 1–2 semanas |

Estimativa total conservadora para uma pessoa: aproximadamente 10–18 semanas em ritmo contínuo, ajustável conforme protótipo e disponibilidade. Recursos podem ser entregues por incrementos ao fim de cada fase.

## Dependências críticas
- Validar OpenCode 2.x antes de fixar versão do SDK ou schema do Bridge.
- Autenticação e autorização **antes** de permitir acesso Android remoto.
- Acesso remoto privado **antes** de enviar comandos do Android.
- Broker polkit **depois** de políticas de aprovação, auditoria e simulador de ações.
- A experiência Linux (atalho, foco, sobreposição) precisa de teste no GNOME Wayland real.

## Critérios de MVP
1. Do atalho Linux à mensagem enviada ao OpenCode; resposta em streaming.
2. Fechar painel preserva execução e permite retomar sessão.
3. Erros/offline/autorização tratados; OpenCode e Bridge sem root.
4. Sem porta pública, credencial hardcoded, senha de sudo ou root genérico.

## Mapa de issues
As issues serão criadas em sequência. Seus IDs e dependências serão incluídos aqui ao finalizar a organização.
