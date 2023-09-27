# Modifying Linux

## Content

· [🇪🇸 Español]
· [🇺🇸 English]

## ESPAÑOL

## Terminal (Kitty)

Vamos a empezar modificando la terminal, en mi caso uso Kitty como terminal.

## Instalando Kitty

Para instalar Kitty tenemos que ejecutar los siguientes comandos

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

## Configurando la terminal

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

## La configuración de mi zsh

```bash
# Copiar
```

[`Foreground en azul y cambia el background`]
[Las palabras en azul]

``` bash
Aqui para poder poner comandos de terminal
```

``` python
print("Aquí podemos poner todo lo que queramos haciendo lo mismo que en bash pero con el lenguaje que queramos")
```
