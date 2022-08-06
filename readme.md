# Mi configuracion de sistema (Manjaro) con i3 

## Entorno de escritorio

Esta configuracion esta basada en **manjaro**, que es derivada de **arch**. Para manejar los repositorios **AUR** uso **yay**. para nuestra terminal usamo **zsh**

### yay (configuracio)

instalacion:

~~~ bash
git clone https://aur.archlinux.org/yay-git.git
sudo mv yay-git /opt/
cd /opt/yay-git
makepkg -si
~~~

y ya tendremos nuestra instalacion de yay en muestro sistema. _(es recomendable usar el directorio "/opt/" para dejar archivos de instalacion)_.

Podeoms ver la version de yay usando `yay --version`.

### zsh (config)

instalamos zsh y la seteamos como nuestra shell por defecto 

~~~ bash
sudo pacman -S zsh
chsh -s /bin/zsh
~~~

Para ver los shells instalados podemos usar `chsh -l`.

Instalamos oh my zsh para personalizar y configurar ciertos atajos.

~~~ bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
~~~

_mi recomendacion de themes para zsh actual es "nanotech" o "jonathan"._

Un atajo que me gusta usar es `alias ab="xdg-open"` para poder abrir archivos y programas desde la terminal.

## Apliaciones

Lista de packetes a instalar.

~~~ bash
sudo pacman -S python-pip git nodejs feh ttf-font-awesome powerline-fonts picom terminator rofi xorg-xinput xclip light base-devel vim neovim xclip lxappearance
~~~

- feh _(aplicacion para asignar fondos de escritorios)_
- picom _(dar transparencia a ventanas)_
- xorg-xinput _(gestion de mausepad)_
- base-devel _(alguas librerias de desarrollo basicas)_
- xclip _(para poder usar el portapapeles del sistema en vim y neovim)_
- light _(manejo de brillo de pantalla)_
- lxappearance _(manejo de temas gtk)_


### Rofi

menu de aplicaciones, seleccionamos el tema con `rofi-theme-selector`. 

el comando que utilizo para ejecutar la ventana esta dentro de un script en la configuracion de i3. `rofi -show drun -show-icons` mapeado en la combinacion [mod+space].

### i3 bumblebee-status-bar

una alterativa simple y persoalizable a las que trae por defecto i3. su intalacion (a traves de los repositorios aur) consiste en:

~~~ bash
git clone https://aur.archlinux.org/bumblebee-status.git
cd bumblebee-status
makepkg -sicr
~~~

una vez instalado, esta estara dentro de la carpeta `/usr/bin/bumblebee-status`, que es la direccion que esta especificada en el archivo de configuracion

### Temas e iconos

uso el tem gtk3 dracula y los iconos de mcmojave-circle-purple, lo descargamos de la pagina de xfce-look, y los archivos una ves descargados y descomprimidos _(tar -xf <archivo>)_ lo guardamos en la carpeta `/usr/share/themes` y `/usr/share/icons`
