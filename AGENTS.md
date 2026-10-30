# AGENTS.md — OpenCode Assistant

Este arquivo é a entrada obrigatória para agentes de programação. O projeto está **em planejamento**. Leia integralmente os documentos vinculados antes de alterar código.

## Objetivo e escopo
Criar um assistente pessoal e de desenvolvimento com UI Flutter para Fedora/GNOME e Android, usando o OpenCode no computador Linux. O app é um **cliente**; o OpenCode é o motor de agentes, não será reimplementado.

## Leitura obrigatória
1. [Arquitetura](.agents/ARCHITECTURE.md)
2. [Segurança e ameaças](.agents/SECURITY.md)
3. [Contrato do Bridge](.agents/API_CONTRACT.md)
4. [UI/UX](.agents/UI_UX.md)
5. [Roadmap](.agents/ROADMAP.md)
6. [Decisões](.agents/DECISIONS.md)
7. [Ambiente e qualidade](.agents/DEVELOPMENT.md)
8. [Ações privilegiadas](.agents/PRIVILEGED_ACTIONS.md)

## Regras inegociáveis
- Nunca solicitar, salvar, registrar, transmitir ou inserir senha sudo/root em prompt, configuração, logs, environment, IPC, QR code ou repositório. Não criar mecanismo de entrega de senha ao LLM.
- OpenCode, Assistant Bridge e clientes nunca executam como root. Um broker isolado poderá oferecer **apenas operações privilegiadas explícitas**, autorizadas por polkit, com argumentos validados e sem shell genérico.
- Nada de `sudo -S`, `echo senha | sudo`, `sudoers NOPASSWD: ALL`, `chmod 777`, `setuid` amplo, `pkexec sh -c` genérico ou desabilitar confirmações por padrão.
- Nunca expor OpenCode/Bridge na internet pública. Bind do OpenCode em loopback; acesso remoto apenas por transporte privado autenticado e pareamento de dispositivos.
- Não executar comandos fornecidos pelo Android diretamente em um shell do PC. Somente mensagens ao OpenCode em sessão autenticada, sujeitas às permissões, ou operações tipadas do broker.
- Preservar configurações e permissões existentes do OpenCode e do usuário; nunca sobrescrever arquivos pessoais automaticamente.
- O OpenCode pode trocar APIs/SDK entre versões. Validar e fixar a versão compatível na issue de investigação antes de codificar o adaptador.
- Separar a lógica do backend e da UI. Flutter compartilha componentes entre Linux e Android; recursos nativos ficam em adaptadores de plataforma.
- Código e UI em inglês, traduções por ARB em pt-BR, en e es; pt-BR como idioma inicial.
- Interface acessível, teclado primeiro no Linux, touch primeiro no Android; claro/escuro e cores do sistema quando possível.
- Não usar dados reais, tokens, senhas, caminhos privados nem capturas pessoais em testes/fixtures ou issues.

## Fluxo de trabalho
1. Selecionar uma issue que esteja desbloqueada e registrar as dependências.
2. Propor a menor alteração útil e documentar a decisão relevante em `.agents/DECISIONS.md`.
3. Implementar com erros tratados, logs sem segredos, cancelamento de tarefas e estados de conexão claros.
4. Para alterações de autenticação, execução remota ou privilégios, incluir testes de caminhos negados e revisão de segurança.
5. Atualizar README/docs, rodar análise estática e testes pertinentes quando houver implementação, e reportar limitações reais.
6. Não marcar uma funcionalidade como pronta sem testar o fluxo real Linux ⇄ Bridge ⇄ OpenCode e, quando aplicável, Android ⇄ Tailscale ⇄ Bridge.

## Convenções previstas
- `apps/client/` Flutter: UI Linux/Android, state management mínimo e adaptadores de plataforma.
- `services/bridge/` Node.js/TypeScript: serviço de usuário, auth, streaming e adaptador do OpenCode.
- `services/privileged-broker/` componente pequeno e isolado para ações tipadas via polkit (fase posterior).
- `packages/contracts/` especificação de API e schemas compartilhados.
- `docs/` instalação e uso; `.agents/` decisões, arquitetura e regras para agentes.

Estrutura sugerida, sujeita à issue de bootstrap; não criar diretórios vazios desnecessários.
