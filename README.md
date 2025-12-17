Autoclicker Ultra Rápido no Linux

Wayland e X11 • CLI • Sem interface gráfica

Este guia mostra como criar um autoclicker extremamente rápido no Linux usando apenas terminal.
Funciona em Wayland e X11, sem interface gráfica e com controle total via mouse.

Comportamento final:

Um clique no botão esquerdo ativa o autoclick

Outro clique desativa

Velocidade máxima possível (sem delay)

────────────────────────────────

1. Instalação das dependências

Atualize o sistema e instale as ferramentas necessárias:

sudo apt update
sudo apt install ydotool libinput-tools

O ydotool é responsável por gerar os cliques e o libinput por capturar o evento do mouse físico.

────────────────────────────────

2. Descobrir o evento do mouse

Você precisa identificar qual evento corresponde ao seu mouse real.

Execute:

sudo libinput list-devices

Procure um dispositivo com:

Capabilities: pointer

Exemplo:

Device: 2.4G Wireless Device
Kernel: /dev/input/event12
Capabilities: pointer

Guarde o caminho /dev/input/eventXX, ele será usado no script.

────────────────────────────────

3. Criar o script do autoclicker

Crie o arquivo no diretório home:

nano holdclick.sh

Cole o conteúdo abaixo no arquivo.
Lembre-se de ajustar o valor de MOUSE_EVENT para o evento correto do seu mouse.

#!/usr/bin/env bash

MOUSE_EVENT="/dev/input/event12"

running=false
click_pid=""

start_clicking() {
(
while true; do
ydotool click 0xC0
done
) &
click_pid=$!
}

stop_clicking() {
if [[ -n "$click_pid" ]]; then
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

4. Tornar o script executável

chmod +x holdclick.sh

────────────────────────────────

5. Iniciar o daemon do ydotool

Este passo é obrigatório.

O ydotool cria um dispositivo de mouse virtual sem kernel, o que exige privilégios de administrador.

Executar:

sudo ydotoold &

Se aparecer um número de processo, o daemon está rodando corretamente.

────────────────────────────────

6. Executar o autoclicker

Início do roteiro:

sudo ./holdclick.sh

O script ficará rodando em silêncio.

Uso:

Clique uma vez com o botão escerdo → clique automático ATIVA

Clique recente → clique automático DESATIVA

────────────────────────────────

7. Parar o autoclicker

Para cerrar o script e o daemon:

sudo pkill -f holdclick.sh
sudo pkill ydotoold

────────────────────────────────

Observações importantes

Velocidade extrema alta (milhas de cliques por segundo)

Alguns jogos ou aplicações podem limitar ou detectar macros

Função em Wayland e X11

Não usa interface gráfica

Baseado em entrada real do kernel

────────────────────────────────

Conclusão

Este é um dos métodos mais rápidos e confiáveis para autoclick no Linux moderno, especialmente em Wayland, onde ferramentas antigas falham.

Possíveis evoluções:

Removedor de uso de sudo com registros udev

Criar uma versão em C ainda mais rápida

Transformar em serviço systemd

Usar tecla do teclado como gato
