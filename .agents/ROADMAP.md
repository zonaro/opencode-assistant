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

Cada issue possui critérios de aceite, estimativa e dependências explícitas. O número é o identificador real no GitHub.

### F0 — Fundação

- [#1](https://github.com/zonaro/opencode-assistant/issues/1) — Compatibilidade OpenCode
- [#2](https://github.com/zonaro/opencode-assistant/issues/2) — Monorepo Flutter/Bridge
- [#3](https://github.com/zonaro/opencode-assistant/issues/3) — Contrato API
- [#4](https://github.com/zonaro/opencode-assistant/issues/4) — CI e qualidade

### F1 — MVP Linux

- [#5](https://github.com/zonaro/opencode-assistant/issues/5) — Bridge e serviço do usuário
- [#6](https://github.com/zonaro/opencode-assistant/issues/6) — Sessões e streaming
- [#7](https://github.com/zonaro/opencode-assistant/issues/7) — Chat Flutter
- [#8](https://github.com/zonaro/opencode-assistant/issues/8) — Overlay GNOME
- [#9](https://github.com/zonaro/opencode-assistant/issues/9) — Modos Pessoal/Desenvolvimento
- [#10](https://github.com/zonaro/opencode-assistant/issues/10) — Aprovações e auditoria
- [#11](https://github.com/zonaro/opencode-assistant/issues/11) — Tema, i18n e acessibilidade

### F2 — Android privado

- [#12](https://github.com/zonaro/opencode-assistant/issues/12) — Pareamento/revogação
- [#13](https://github.com/zonaro/opencode-assistant/issues/13) — Tailscale Serve
- [#14](https://github.com/zonaro/opencode-assistant/issues/14) — Cliente Android
- [#15](https://github.com/zonaro/opencode-assistant/issues/15) — Retomada multi-dispositivo

### F3 — Experiência

- [#16](https://github.com/zonaro/opencode-assistant/issues/16) — STT/TTS
- [#17](https://github.com/zonaro/opencode-assistant/issues/17) — Notificações
- [#18](https://github.com/zonaro/opencode-assistant/issues/18) — Anexos/clipboard

### F4 — Admin seguro

- [#19](https://github.com/zonaro/opencode-assistant/issues/19) — Broker polkit
- [#20](https://github.com/zonaro/opencode-assistant/issues/20) — Operações administrativas tipadas
- [#21](https://github.com/zonaro/opencode-assistant/issues/21) — Hardening e bloqueio emergencial

### F5 — Distribuição

- [#22](https://github.com/zonaro/opencode-assistant/issues/22) — RPM/AppImage Fedora
- [#23](https://github.com/zonaro/opencode-assistant/issues/23) — APK + validação ponta a ponta

**Primeira execução recomendada:** #1 → #2 → #3 → #5 → #6 → #7 → #8. A issue #4 pode ocorrer em paralelo depois de #2. As demais seguem as dependências individuais. A fase F4 é opcional para um assistente sem privilégios, mas obrigatória antes de disponibilizar ações administrativas reais.
