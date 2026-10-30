# Segurança e modelo de ameaças

**Regra central:** a frase \"guarde minha senha de sudo para o agente usar\" não é requisito a implementar literalmente. O objetivo legítimo é executar ações administrativas autorizadas com poucos prompts; a senha **nunca** entra em mãos do agente/Bridge/cliente.

## Ativos
- Conta e arquivos Linux, repositórios, tokens de providers/MCP, comandos/sessões do OpenCode.
- Rede doméstica e acesso via celular; chaves de pareamento; operações administrativas do PC.

## Ameaças e controles

| Ameaça | Controle obrigatório |
|---|---|
| Android perdido/roubado | tokens por dispositivo em Android Keystore, desbloqueio/biometria opcional, revogação imediata, expiração e trilha |
| Rede hostil/replay | HTTPS Tailscale Serve, credencial por dispositivo, nonce/idempotência em ações, rate limit |
| Prompt injection em documentos/sites | permissões do OpenCode, escopos por projeto, confirmação explícita de operação sensível, nunca interpretar texto do modelo como autorização |
| Agente solicita root | broker de operação tipada, allowlist e validação de parâmetros, sem caminho arbitrário/shell root |
| Elevação via argumentos | binários e argumentos definidos pelo broker, proibir `sh -c`/composição de shell, caminhos canônicos quando aplicável |
| Vazamento de segredos | não registrar prompts com tokens em logs; redaction; permissões de arquivo; nunca expor credencial do OpenCode ao cliente |
| Dispositivo de rede comprometido | pareamento presencial no desktop, tokens distintos, desconectar todos, modo somente leitura por padrão no primeiro pareamento |
| Confirmação falsa/duplicada | aprovação vinculada à operação exata, sessão, hash de argumentos, timestamp e validade curta |
| PC sem usuário presente | negar ações de alto risco por padrão; opções restritas explícitas para ações de baixo risco |

## Matriz de ações
- **Consulta**: leitura de status e conversa; sem confirmação adicional se autenticado.
- **Alteração de usuário**: edição de projeto/comando não-root sob permissões OpenCode; respeitar confirmação do OpenCode, com UI remota.
- **Potencialmente destrutiva**: exclusão, alteração de grandes árvores, rede, chaves ou atualização crítica; confirmação explícita.
- **Privilegiada**: **somente** operação tipada do broker com política por ação. Ações perigosas mantêm desafio do sistema local; não há botão global \"sempre permitir sudo\".

## Pareamento
1. PC gera desafio de uso único com curta validade, mostrado localmente por QR/código.
2. Android estabelece sessão por HTTPS privado e apresenta solicitação ao usuário local.
3. Bridge registra chave/token único por dispositivo, nome e escopos mínimos; segredo guardado no Keystore/Secret Service.
4. Tokens rotacionáveis/revogáveis, sem impressão em logs ou QR permanente; não depender apenas da identidade Tailscale.
5. Perda do celular: revogar token no PC; log e desligamento de acesso remoto devem funcionar mesmo sem internet.

## Políticas de rede
- OpenCode e Bridge somente no loopback; Tailscale Serve permite acesso privado por HTTPS à Bridge; **não** usar Funnel, port-forward público, UPnP ou `0.0.0.0` por padrão.
- Nenhuma ação destrutiva deve ser repetida automaticamente após timeout.
- Documentar riscos residuais: uma conta normal comprometida ou um agente com shell não-root ainda podem acessar os arquivos que esse usuário consegue acessar.

## Auditoria
Registrar timestamp, dispositivo, sessão, operação, alvo não sensível, decisão, resultado e ID correlacionado. Não gravar senhas, tokens, headers Authorization, conteúdo de arquivos pessoais ou saída integral de comandos por padrão.

## Critérios para liberação
Cenários de teste incluem MITM simples, dispositivo revogado, prompt injection, pedido arbitrário de root, path traversal, replay, aprovação expirada e interrupção de rede. Revisão manual obrigatória das políticas de polkit/sudoers antes de distribuir.
