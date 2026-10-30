# Desenvolvimento, estrutura e validação

## Status
Repositório em fase de planejamento. Nada deve ser apresentado como compilável até o scaffold existir.

## Estrutura de destino
```text
apps/client/                 # Flutter, Linux e Android
services/bridge/             # TypeScript, OpenCode adapter, API, auth
services/privileged-broker/  # serviço pequeno e separado, fase posterior
packages/contracts/          # OpenAPI e schemas
docs/                        # guias de uso, instalação, troubleshooting
.agents/                     # instruções e arquitetura
AGENTS.md
README.md
```

## Convenções
- Dart com `flutter analyze`, `dart format`, widgets e serviços testáveis.
- TypeScript em modo strict, lint/testes de contrato, validação de entrada em runtime; versões fixadas em lockfile.
- Adotar apenas dependências justificadas. Preferir padrão simples e reutilizável.
- Sem bibliotecas de comandos root genéricos, sem manipular config sudoers/polkit silenciosamente.
- Segredos somente nos repositórios seguros do SO quando inevitáveis (tokens de dispositivo, **não** sudo), nunca no Git.
- Não capturar/mirrorar arquivos ou histórico de terceiros sem ação explícita do usuário.

## Verificação por fase
1. Compatibilidade: OpenCode server/SDK atual, criação de sessão, prompt, streaming, cancelamento, autorização.
2. Bridge: simulações de OpenCode offline, token inválido/revogado, replay, conexão intermitente e grandes respostas.
3. Flutter Linux: inicialização, atalhos no GNOME Wayland, `Esc`, foco, notificação, overlay e reconexão.
4. Android: pareamento, retomada após troca de Wi-Fi/celular, app em background, biometria se habilitada.
5. Transporte: acesso somente pela tailnet, TLS, nenhum bind público, desativação/revogação funcional.
6. Broker: somente ações allowlisted, parâmetros maliciosos rejeitados, identidade real verificada, polkit negado respeitado.
7. Entrega: instaladores, configuração inicial, rollback de serviço e desinstalação sem apagar dados sem consentimento.

## Empacotamento
- Fedora prioritário: RPM e AppImage se compatíveis com integração GNOME; posteriormente DEB.
- Android: APK para testes locais, assinatura e pipeline reprodutível na fase release.
- Bridge instalado como `systemd --user`, logs reduzidos, configuração XDG e migração versionada.
- README exibe comandos somente após efetivamente validados.

## Testes e aprovação
- Nenhum teste destrutivo em máquina real, updates do SO, desligamento ou alteração de políticas sem consentimento específico.
- Fixtures, simulador de broker e teste de integração com OpenCode rodando como usuário comum.
- Documentar explicitamente o que foi e não foi testado em cada PR.
