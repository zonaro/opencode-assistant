# Ações privilegiadas (proposta para fase posterior)

## Problema
O usuário quer dizer \"atualize meu sistema\" do Android ou Linux sem digitar a senha do sudo repetidamente. **Nunca armazenar a senha** em Flutter secure storage, keyring, arquivo, stdin do processo ou contexto OpenCode: cifrar não torna seguro entregá-la ao agente.

## Solução
Criar um **broker mínimo e separado** do Bridge, com API local de operações explícitas. O processo que conversa com OpenCode continua sem root. Para cada operação, broker:
1. Identifica chamador e verifica autenticação local (não confiar em UID informado no JSON).
2. Valida nome de operação e parâmetros contra schema e lista de valores permitidos.
3. Consulta autorização polkit correspondente àquela ação (não uma autorização root genérica).
4. Exige confirmação na UI quando aplicável, vinculada ao hash dos argumentos.
5. Executa binário fixo com argv estruturado, sem shell, ambiente controlado, timeout, limites de saída e auditoria.
6. Retorna status; nunca devolve segredos ou descritores privilegiados reutilizáveis.

## Exemplos candidatos — sempre opt-in
| ID lógico | Ação | Política inicial |
|---|---|---|
| `system.read_status` | status do sistema (sem root) | permitido, sem broker |
| `system.update_packages` | atualização via mecanismo suportado pela distro | confirmação por execução; política polkit específica |
| `service.restart_allowed` | reiniciar serviço de lista cadastrada | lista estrita e confirmação; negar serviço arbitrário |
| `system.reboot` | reiniciar PC | confirmação explícita e user-presence; nunca executar silenciosamente |

Operações **proibidas**: `execute_root_command(command)`, `sudo_anything`, escrever qualquer caminho como root, editar sudoers/polkit via agente, shell root persistente, desativar firewall ou permissão universal.

## Sem prompt de senha — limites reais
Para ações de baixo risco e explicitamente aprovadas na instalação, uma política polkit **muito específica** pode permitir sem senha. Para alterações perigosas, exigir autenticação local ou desafio de presença. Se houver necessidade de aprovação administrativa interativa, isso é uma etapa de segurança, não um bug.

## Cuidados de implementação
- Polkit controla autorização; as verificações de parâmetros devem ficar no código privilegiado também.
- Não usar `pkexec sh -c`, concatenar strings em comandos ou aceitar caminhos sem canonicalização/escopo.
- Preferir interfaces do sistema (D-Bus/systemd/PackageKit) quando apropriadas e seus próprios mecanismos polkit a wrappers root.
- Se o broker falhar, o assistente continua funcionando sem privilégios.
- Não instalar/modificar regras polkit automaticamente sem mostrar exatamente o que será alterado e obter confirmação explícita do usuário.
