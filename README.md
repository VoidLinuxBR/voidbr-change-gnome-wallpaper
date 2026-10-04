<div align="center">

# 🔵 voidbr-change-gnome-wallpaper

**Slideshow automático de wallpapers para o GNOME (usa os wallpapers já instalados), com opção de deixar um **wallpaper fixo** e uma interface gráfica para configurar tudo.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](LICENSE)

</div>

---

Feito para o [VoidBR Linux](https://voidbr.org).

## Como funciona

| Arquivo | Função |
|---|---|
| `/etc/xdg/autostart/voidbr-change-gnome-wallpaper.desktop` | Inicia o timer no login do GNOME |
| `/usr/local/bin/voidbr-change-gnome-wallpaper-timer` | Laço que lê o `.conf` e chama o script a cada intervalo (uma instância por usuário) |
| `/usr/local/bin/voidbr-change-gnome-wallpaper` | Aplica o próximo wallpaper da pasta (ou uma imagem passada como argumento) |
| `/usr/local/bin/voidbr-change-gnome-wallpaper-gui` | Interface gráfica (GTK4) para editar o `.conf` |
| `/etc/voidbr-change-gnome-wallpaper.conf` | Configuração |

Os wallpapers do slideshow vêm da pasta definida em `WALLPAPER_DIR` (padrão:
`/usr/share/backgrounds/chililinux`). Só as imagens entram no slideshow (`.jpg`, `.jpeg`,
`.png`, `.webp`, `.svg`, `.bmp`, `.jxl`, `.avif`); outros arquivos da pasta são ignorados.
A posição atual fica em `~/.cache/voidbr-change-gnome-wallpaper/`, então cada usuário tem a sua sequência.

O timer confere o `.conf` a cada 5 segundos: quando ele muda, a nova configuração vale
**na hora**, sem reiniciar nada e sem esperar o fim do intervalo antigo.

Depois de instalar o pacote, o timer inicia sozinho no próximo login do GNOME. Para
iniciar na sessão atual, rode `voidbr-change-gnome-wallpaper-timer &` ou use o botão
**▶️ Iniciar timer** da interface gráfica.

## Configuração

`/etc/voidbr-change-gnome-wallpaper.conf`:

```sh
# Pasta com os wallpapers do slideshow
# vazio = /usr/share/backgrounds/chililinux
WALLPAPER_DIR=/usr/share/backgrounds/chililinux

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
| `WALLPAPER_DIR` | caminho de uma pasta, ou vazio | Pasta usada no slideshow. Vazio (ou ausente, em `.conf` antigos) usa `/usr/share/backgrounds/chililinux`. |
| `INTERVAL` | número de segundos | Tempo entre as trocas. Valor inválido vira 300 (5 min). |
| `DISABLED` | `yes`, `true`, `1` desativam; qualquer outro valor (ou ausente) ativa | Com `yes`, o timer continua rodando mas não troca o wallpaper. |
| `FIXED_WALLPAPER` | caminho de uma imagem, ou vazio | Se preenchido e o arquivo existir, mantém sempre essa imagem em vez do slideshow. Se o arquivo sumir, volta ao slideshow. |

Caminhos com espaço precisam de aspas:

```sh
WALLPAPER_DIR='/usr/share/backgrounds/Meus Wallpapers'
FIXED_WALLPAPER='/usr/share/backgrounds/Meus Wallpapers/praia.png'
```

> O `.conf` é global: use pastas e imagens fora do `$HOME` para que valham para todos os usuários.

## Interface gráfica

```sh
voidbr-change-gnome-wallpaper-gui
```

Ou pelo menu do GNOME: **VoidBR Wallpaper Slideshow**.

- **🎞️ Slideshow:** escolhe o intervalo em segundos, minutos ou horas.
- **📌 Wallpaper fixo:** escolhe a imagem pelas miniaturas da pasta ou em **📂 Outro arquivo…**.
  Ao salvar, o wallpaper é aplicado na hora.
- **⏸️ Desativado:** para as trocas sem perder as outras configurações.
- **📁 Pasta…:** escolhe a pasta dos wallpapers (`WALLPAPER_DIR`); as miniaturas mudam na hora.
- **⏭️ Próximo:** pula para o próximo wallpaper do slideshow.
- **♻️ Padrão:** volta para slideshow a cada 1 minuto, na pasta padrão.
- **▶️ Iniciar timer:** aparece quando o timer está parado e o inicia na sessão atual.

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

`bash`, `gnome-shell`, `util-linux` (`flock`) e, para a interface gráfica, `python3`,
`python3-gobject`, `gtk4` e `polkit`.

A interface usa o renderizador cairo (sem OpenGL/Vulkan), então roda em qualquer máquina
ou VM. Para usar outro renderizador, defina `GSK_RENDERER` antes de abrir o app.

## Licença

MIT — Vilmar Catafesta <vcatafesta@gmail.com>. Veja o arquivo [LICENSE](LICENSE).
