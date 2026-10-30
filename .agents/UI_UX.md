# UI/UX — aplicativo compacto de assistência

## Direção visual
- Estilo Adwaita/flat, limpo, sem excesso de chrome; cores do sistema quando viável.
- Tema claro/escuro/automático, acessibilidade, fontes escaláveis, pt-BR/en/es.
- Estados honestos: conectado, offline, gerando, aguardando permissão, tarefa concluída, erro recuperável.
- Interface do assistente pessoal deve ser mais simples que um IDE, mas não esconder autorizações relevantes.

## Linux — painel rápido
- Atalho global **configurável** abre painel compacto próximo ao centro/topo, já com foco no campo de entrada; pressionar de novo pode ocultar.
- `Esc` oculta sem cancelar tarefa; botão **Cancelar tarefa** é separado. Fechar janela não desliga Bridge.
- Expandir para histórico/visão completa sem perder texto, anexos ou sessão.
- Entrada com multiline, Enter enviar / Shift+Enter nova linha (configurável), push-to-talk, seletor de modo Pessoal/Projeto, indicador de modelo/agente.
- Mostra ações relevantes, resposta legível, copiar e feedback visual não intrusivo.
- Suporte a bandeja/notificações quando integrações GNOME permitirem.
- **GNOME Wayland:** não supor suporte universal a always-on-top, foco forçado ou atalho global de aplicativo. Preferir integração do desktop/portal suportado; oferecer atalho personalizado nas Configurações do GNOME e documentar fallback. Testar GNOME nativo.
- Sem manipulação por automação invasiva do display/X11 para contornar Wayland.

## Android
- Início com conversas recentes, campo de comando, botão de voz e estado do PC.
- Pareamento por QR/código gerado no Linux e tela clara de permissões do aparelho.
- Histórico e streaming com reconexão; notificações de tarefa finalizada; seleção de sessão.
- Pedidos de aprovação apresentam **operação, alvo, argumentos relevantes e risco**, com `Permitir esta vez` / `Negar`. Biometria opcional para confirmar ações sensíveis, sem substituir polkit quando exigido.
- Sem exposição de comandos de shell privilegiados em botões genéricos.

## Voz
1. Fase inicial: segurar/tocar para falar, transcrição, revisar texto e enviar.
2. Resposta falada (TTS) opcional e interrupção segura.
3. Fase futura: conversação contínua, detecção de silêncio, supressão de eco e wake word **opt-in**. Nenhuma gravação contínua em background por padrão.
4. Transcrição local no PC quando possível, com informação transparente ao usuário se áudio circular pela rede.

## Experiências-chave
- Pressionar atalho → pedir \"encontre o erro do aplicativo\" → ler streaming → Esc → notificação no fim → reabrir na mesma sessão.
- Abrir Android fora de casa → ver PC conectado via Tailscale → pedir status → acompanhar tarefa.
- Solicitar \"reinicie serviço permitido\" → UI apresenta aprovação → broker verifica política → status e auditoria.
- PC desligado → UI informa \"PC offline\" e permite tentar novamente, sem afirmar que executou nada.
