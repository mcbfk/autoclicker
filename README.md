────────────────────────────────
AUTOCLICKER ULTRA RÁPIDO NO LINUX
Wayland e X11 • CLI • Sem interface gráfica
────────────────────────────────

Este tutorial ensina a criar um autoclicker extremamente rápido no Linux usando apenas terminal. Ele funciona tanto em Wayland quanto em X11 e não depende de interface gráfica.

Comportamento final:
– Um clique não pode ser ativado ou autoclick
– Outro clique desativa
– Velocidade máxima possível (sem atraso)

────────────────────────────────

INSTALAÇÃO DAS DEPENDÊNCIAS
────────────────────────────────

Atualize o sistema e instale como ferramentas necessárias:

atualização sudo apt
sudo apt install ydotool libinput-tools

O ydotool será responsável por gerar os cliques e o libinput por detectar o botão do mouse real.

────────────────────────────────
2) DESCUBRIR O EVENTO DO MOUSE
────────────────────────────────

Agora você precisa identificar qual evento do sistema corresponde ao seu mouse físico.

Executar:

sudo libinput lista-dispositivos

Procure um dispositivo que tenha:

Capacidades: ponteiro

Exemplo válido:

Dispositivo: Dispositivo sem fio 2.4G
Kernel: /dev/input/event12
Capacidades: ponteiro

Ignore dispositivos que sejam apenas teclado, controle do consumidor ou gesto.

Guarda o caminho do Kernel (exemplo: /dev/input/event12).

────────────────────────────────
3) CRIAR O SCRIPT DO AUTOCLICKER
────────────────────────────────

Crie o arquivo no seu diretório pessoal:

nano holdclick.sh

Cole o conteúdo baixo dentro do arquivo.
Atenção: substituir/dev/input/event12 pelo evento correto do seu mouse.

#!/usr/bin/env bash

MOUSE_EVENT="/dev/input/event12"

correndo=falso
clique_pid=""

iniciar_clicar() {
(
while true; do
ydotool click 0xC0
done
) &
click_pid=$!
}

stop_clicking() {
se [[ -n "$click_pid" ]]; entidade
matar "$click_pid" 2>/dev/null
clique_pid=""
fi
}

libinput debug-events --device "$MOUSE_EVENT" | enquanto a linha -r; faça
se echo "$line" | grep -q "BTN_LEFT.*pressionado"; então
se! $em execução; entrada
executando=verdadeiro
iniciar_clicando
outro
correto=falso
parar_clicando
fi
fi
feto

Salve com Ctrl + O, Enter
Saia com Ctrl + X

────────────────────────────────
4) PERMISSÃO DE EXECUÇÃO
────────────────────────────────

Torne o script executável:

chmod +x holdclick.sh

────────────────────────────────
5) INICIAR O DAEMON DO YDOTOOL
────────────────────────────────

Este passo é obrigatório.

O ydotool precisa criar um dispositivo de mouse virtual sem kernel, e é permitido com privilégios de administrador.

Executar:

sudo ydotoold &

Se aparecer um número de processo, o daemon está rodando corretamente.

────────────────────────────────
6) EXECUTOR O AUTOCLICKER
────────────────────────────────

Agora inicial o script:

sudo ./holdclick.sh

O script ficará rodando em silêncio no terminal.

Uso:
– Clique uma vez com o botão escerdo → clique automático ATIVO
– Clique recente → clique automático DESATIVA

Não é necessário segurar o botão.

────────────────────────────────
7) COMO PARAR TUDO
────────────────────────────────

Para cerrar o script e o daemon:

sudo pkill -f holdclick.sh
sudo pkill ydotoold

────────────────────────────────
OBSERVAÇÕES IMPORTANTES
────────────────────────────────

– A velocidade é extremamente alta (milhas de cliques por segundo)
– Alguns jogos ou programas podem limitar ou detectar esse comportamento
– Função em Wayland e X11
– Não depende de interface gráfica
– Usa input real do kernel, não simulação gráfica

────────────────────────────────
CONCLUSÃO
────────────────────────────────

Esse método é atualizado ou mais confiável e rápido para autoclick no Linux moderno, especialmente em Wayland, onde ferramentas antigas como xdotool falham.

Se quiser evoluir depois, é possível:
– removedor o uso de sudo com registros de udev
– criar uma versão em C ainda mais rápida
– transformar em serviço systemd
– trocar o gato do rato por tecla do tecido
