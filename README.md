# Modificando Linux

## Contenido

· 🇪🇸 Español

## ESPAÑOL

Tened en cuenta que tenéis todos los archivos de ese proyecto por si preferís descargarlo. Y todavía no he añadido ninguna configuración sobre mi Linux, actualmente uso Ubuntu, pero no descarto en ningún momento en cambiarme a alguna otra distribución, pero de momento quiero seguir usando Ubuntu porque soy nuevo en esto y prefiero ir cogiendo practica actualmente, cuando haga cambios en mi Linux actualizaré por aquí.

## Terminal (Kitty)

Vamos a empezar modificando la terminal, en mi caso uso Kitty como terminal. Podéis utilizar la terminal con la que mejor os sintáis.

## Instalando Kitty

Para instalar Kitty tenemos que ejecutar los siguientes comandos.

``` bash
curl -L <https://sw.kovidgoyal.net/kitty/installer.sh> | sh /dev/stdin

# En caso de que no tengas creado un archivo en ~/.local/bin
mkdir ~/.local/bin

# Creando enlaces simbolicos para agregar kitty y kitten (asumiendo que ~/.local/bin está
# en tu sistema)
ln -sf ~/.local/kitty.app/bin/kitty ~/.local/kitty.app/bin/kitten ~/.local/bin/

# Coloca el archivo kitty.desktop en algún lado para que pueda ser encontrado por el SO
cp ~/.local/kitty.app/share/applications/kitty.desktop ~/.local/share/applications/

# Si quieres poder abrir el archivo buscando por su nombre o imagen tienes que añadir kitty-open.desktop
cp ~/.local/kitty.app/share/applications/kitty-open.desktop ~/.local/share/applications/

# Actualiza los archivos de kitty y el icono de kitty.desktop
sed -i "s|Icon=kitty|Icon=/home/$USER/.local/kitty.app/share/icons/hicolor/256x256/apps/kitty.png|g" ~/.local/share/applications/kitty*.desktop

sed -i "s|Exec=kitty|Exec=/home/$USER/.local/kitty.app/bin/kitty|g" ~/.local/share/applications/kitty*.desktop
```

## Configurando la zsh

En la terminal usaremos la *zsh*. Para poder usar la zsh en tu terminal tienes que hacer lo siguiente.

``` bash
sudo apt update

sudo apt install zsh

# Ejecutamos este comando, para poder entrar en el modo zsh de la terminal y apartir de ahora poder configurar todo 
zsh

# Hacemos que la terminal sea zsh por predeterminado
chsh -s /usr/bin/zsh

# Aprovechamos y instalamos Oh My ZSH que nos servira para poder instalar zsh-autosuggestions y zsh-syntax-highliting
git clone https://github.com/ohmyzsh/ohmyzsh.git

# Ahora instalamos zsh-autosuggestions y zsh-syntax-highliting para tenerlo todo automaticamente
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH/plugins/zsh-syntax-highlighting

# Comprobamos que tengamos los plugins bien instalados y los buscamos por su nombre
ls $ZSH/plugins
```

## Instalando la powerlevel10k

```bash
# Primero instalamos la powerlevel10k
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc

# Y para poder activar la powerlevel10k tenemos que ejecutar zsh
zsh
```

## La configuración de mi zsh

```bash
# Copiar esto dentro de 
cd 

nano ~/.zshrc

# Aquí la configuración de mi zsh

# Fix the Java Problem
export _JAVA_AWT_WM_NONREPARENTING=1
export ZSH=$HOME/ohmyzsh

# Enable Powerlevel10k instant prompt. Should stay at the top of ~/.zshrc.
if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
  source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi

# Set up the prompt

autoload -Uz promptinit
promptinit
prompt adam1

setopt histignorealldups sharehistory

# Use emacs keybindings even if our EDITOR is set to vi
bindkey -e

# Keep 1000 lines of history within the shell and save it to ~/.zsh_history:
HISTSIZE=1000
SAVEHIST=1000
HISTFILE=~/.zsh_history

# Use modern completion system
autoload -Uz compinit
compinit

zstyle ':completion:*' auto-description 'specify: %d'
zstyle ':completion:*' completer _expand _complete _correct _approximate
zstyle ':completion:*' format 'Completing %d'
zstyle ':completion:*' group-name ''
zstyle ':completion:*' menu select=2
eval "$(dircolors -b)"
zstyle ':completion:*:default' list-colors ${(s.:.)LS_COLORS}
zstyle ':completion:*' list-colors ''
zstyle ':completion:*' list-prompt %SAt %p: Hit TAB for more, or the character to insert%s
zstyle ':completion:*' matcher-list '' 'm:{a-z}={A-Z}' 'm:{a-zA-Z}={A-Za-z}' 'r:|[._-]=* r:|=* l:|=*'
zstyle ':completion:*' menu select=long
zstyle ':completion:*' select-prompt %SScrolling active: current selection at %p%s
zstyle ':completion:*' use-compctl false
zstyle ':completion:*' verbose true

zstyle ':completion:*:*:kill:*:processes' list-colors '=(#b) #([0-9]#)*=0=01;31'
zstyle ':completion:*:kill:*' command 'ps -u $USER -o pid,%cpu,tty,cputime,cmd'
source /home/zark90s/powerlevel10k/powerlevel10k.zsh-theme

# To customize prompt, run `p10k configure` or edit ~/.p10k.zsh.
[[ -f ~/.p10k.zsh ]] && source ~/.p10k.zsh

# Manual configuration

PATH=/root/.local/bin:/snap/bin:/usr/sandbox/:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/usr/share/games:/usr/local/sbin:/usr/sbin:/sbin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games

# Manual aliases
alias ll='lsd -lh --group-dirs=first'
alias la='lsd -a --group-dirs=first'
alias l='lsd --group-dirs=first'
alias lla='lsd -lha --group-dirs=first'
alias ls='lsd --group-dirs=first'
alias cat='bat'

[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh

# Plugins
plugins=(git z zsh-autosuggestions zsh-syntax-highlighting)
source $ZSH/oh-my-zsh.sh

# Functions
function mkt(){
 mkdir {nmap,content,exploits,scripts}
}

# Extract nmap information
function extractPorts(){
 ports="$(cat $1 | grep -oP '\d{1,5}/open' | awk '{print $1}' FS='/' | xargs | tr ' ' ',')"
 ip_address="$(cat $1 | grep -oP '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}' | sort -u | head -n 1)"
 echo -e "\n[*] Extracting information...\n" > extractPorts.tmp
 echo -e "\t[*] IP Address: $ip_address"  >> extractPorts.tmp
 echo -e "\t[*] Open ports: $ports\n"  >> extractPorts.tmp
 echo $ports | tr -d '\n' | xclip -sel clip
 echo -e "[*] Ports copied to clipboard\n"  >> extractPorts.tmp
 cat extractPorts.tmp; rm extractPorts.tmp
}

# Set 'man' colors
function man() {
    env \
    LESS_TERMCAP_mb=$'\e[01;31m' \
    LESS_TERMCAP_md=$'\e[01;31m' \
    LESS_TERMCAP_me=$'\e[0m' \
    LESS_TERMCAP_se=$'\e[0m' \
    LESS_TERMCAP_so=$'\e[01;44;33m' \
    LESS_TERMCAP_ue=$'\e[0m' \
    LESS_TERMCAP_us=$'\e[01;32m' \
    man "$@"
}

# fzf improvement
function fzf-lovely(){

 if [ "$1" = "h" ]; then
  fzf -m --reverse --preview-window down:20 --preview '[[ $(file --mime {}) =~ binary ]] &&
                  echo {} is a binary file ||
                  (bat --style=numbers --color=always {} ||
                   highlight -O ansi -l {} ||
                   coderay {} ||
                   rougify {} ||
                   cat {}) 2> /dev/null | head -500'

 else
         fzf -m --preview '[[ $(file --mime {}) =~ binary ]] &&
                          echo {} is a binary file ||
                          (bat --style=numbers --color=always {} ||
                           highlight -O ansi -l {} ||
                           coderay {} ||
                           rougify {} ||
                           cat {}) 2> /dev/null | head -500'
 fi
}

function rmk(){
 scrub -p dod $1
 shred -zun 10 -v $1
}

# Finalize Powerlevel10k instant prompt. Should stay at the bottom of ~/.zshrc.
(( ! ${+functions[p10k-instant-prompt-finalize]} )) || p10k-instant-prompt-finalize
source ~/powerlevel10k/powerlevel10k.zsh-theme
source ~/powerlevel10k/powerlevel10k.zsh-theme

source ~/.nvm/nvm.sh
```

## Configurando la terminal (kitty)

Esto nos ayudara a tener una kitty bastante más amigable con la interfaz gráfica. La configuración de la terminal la dejemos para el final.

### Archivo color.ini

```bash
# Aquí creamos directamente el archivo y vamos a poder modificarlo
nano ~/.config/kitty/color.ini

# Insertamos todo este código
cursor_shape          Underline
cursor_underline_thickness 1
window_padding_width  20

# Special
foreground #a9b1d6
background #1a1b26

# Black
color0 #414868
color8 #414868

# Red
color1 #f7768e
color9 #f7768e

# Green
color2  #73daca
color10 #73daca

# Yellow
color3  #e0af68
color11 #e0af68

# Blue
color4  #7aa2f7
color12 #7aa2f7

# Magenta
color5  #bb9af7
color13 #bb9af7

# Cyan
color6  #7dcfff
color14 #7dcfff

# White
color7  #c0caf5
color15 #c0caf5

# Cursor
cursor #c0caf5
cursor_text_color #1a1b26

# Selection highlight
selection_foreground #7aa2f7
selection_background #28344a
```

### Archivo kitty.conf

```bash
# Hacemos lo mismo que antes
nano ~/.config/kitty/color.ini

# Insertamos el código dentro del archivo
enable_audio_bell no

include color.ini

font_family      HackNerdFont
font_size 13

disable_ligatures never

url_color #61afef

url_style curly

map ctrl+left neighboring_window left
map ctrl+right neighboring_window right
map ctrl+up neighboring_window up
map ctrl+down neighboring_window down

cursor_shape beam
cursor_beam_thickness 1.8

mouse_hide_wait 3.0
detect_urls yes

repaint_delay 10
input_delay 3
sync_to_monitor yes

tap_bar_style powerline
inactive_tab_background #e06c75
active_tab_background #98c379
inactive_tab_foreground #000000
tab_bar_margin_color black

map ctrl+shift+enter new_window_with_cwd
map ctrl+shift+t new_tab_with_cwd

background_opacity 0.95

shell zsh

```

## Si quereis que haga un tutorial en YouTube o que preste más configuraciones no dudeis en dejar vuestra estrella. Podeis contactar conmigo por Twitter <3
