<div align="center">

# 🔵 voidbr-change-gnome-wallpaper

**Slideshow automático de wallpapers para o GNOME (usa os wallpapers já instalados), com opção de deixar um **wallpaper fixo** e uma interface gráfica para configurar tudo.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](LICENSE)

</div>

---

Slideshow automático de wallpapers para o GNOME (usa os wallpapers já instalados),
com opção de deixar um **wallpaper fixo** e uma interface gráfica para configurar tudo.

Feito para o [VoidBR Linux](https://voidbr.org).

## Como funciona

| Arquivo | Função |
|---|---|
| `/etc/xdg/autostart/voidbr-change-gnome-wallpaper.desktop` | Inicia o timer no login do GNOME |
| `/usr/local/bin/voidbr-change-gnome-wallpaper-timer` | Laço que relê o `.conf` a cada ciclo e chama o script |
| `/usr/local/bin/voidbr-change-gnome-wallpaper` | Aplica o próximo wallpaper da pasta (ou uma imagem passada como argumento) |
| `/usr/local/bin/voidbr-change-gnome-wallpaper-gui` | Interface gráfica (GTK4) para editar o `.conf` |
| `/etc/voidbr-change-gnome-wallpaper.conf` | Configuração |

Os wallpapers do slideshow vêm de `/usr/share/backgrounds/chililinux`. A posição atual
fica em `~/.cache/voidbr-change-gnome-wallpaper/`, então cada usuário tem a sua sequência.

Como o timer relê o `.conf` a cada ciclo, **não é preciso reiniciar nada** depois de
alterar a configuração: a mudança vale a partir do próximo ciclo.

## Configuração

`/etc/voidbr-change-gnome-wallpaper.conf`:

```sh
# Intervalo em segundos
INTERVAL=60

# Controle do slideshow
# yes | no | true | false | 0 | 1
DISABLED=no

# Wallpaper fixo (caminho completo da imagem)
# vazio = slideshow com os wallpapers da pasta
FIXED_WALLPAPER=
```

| Chave | Valores | Efeito |
|---|---|---|
| `INTERVAL` | número de segundos | Tempo entre as trocas. Valor inválido vira 300 (5 min). |
| `DISABLED` | `yes`, `true`, `1` desativam; qualquer outro valor ativa | Com `yes`, o timer continua rodando mas não troca o wallpaper. |
| `FIXED_WALLPAPER` | caminho de uma imagem, ou vazio | Se preenchido e o arquivo existir, mantém sempre essa imagem em vez do slideshow. Se o arquivo sumir, volta ao slideshow. |

Caminhos com espaço precisam de aspas:

```sh
FIXED_WALLPAPER='/usr/share/backgrounds/Meus Wallpapers/praia.png'
```

> O `.conf` é global: use imagens fora do `$HOME` para que valham para todos os usuários.

## Interface gráfica

```sh
voidbr-change-gnome-wallpaper-gui
```

Ou pelo menu do GNOME: **VoidBR Wallpaper Slideshow**.

- **🎞️ Slideshow:** escolhe o intervalo em segundos, minutos ou horas.
- **📌 Wallpaper fixo:** escolhe a imagem pelas miniaturas da pasta ou em **📂 Outro arquivo…**.
  Ao salvar, o wallpaper é aplicado na hora.
- **⏸️ Desativado:** para as trocas sem perder as outras configurações.
- **⏭️ Próximo:** pula para o próximo wallpaper do slideshow.

A barra de status mostra se o timer está rodando e o que está salvo no `.conf`.
Salvar pede a senha de administrador (pkexec), porque o arquivo fica em `/etc`.

## Linha de comando

```sh
# próximo wallpaper do slideshow
voidbr-change-gnome-wallpaper

# aplica uma imagem específica (não mexe na posição do slideshow)
voidbr-change-gnome-wallpaper /usr/share/backgrounds/chililinux/wallpaper.jpg
```

## Dependências

`bash`, `gnome-shell` e, para a interface gráfica, `python3-gobject`, `gtk4` e `polkit`.

## Licença

MIT — Vilmar Catafesta <vcatafesta@gmail.com>
