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

uso el tem gtk3 juno-mirage y los iconos de mcmojave-circle-purple, lo descargamos de la pagina de xfce-look, y los archivos una ves descargados y descomprimidos _(tar -xf <archivo>)_ lo guardamos en la carpeta `/usr/share/themes` y `/usr/share/icons`

## editor de codigo Vim y neoVim

para instalarlo en distros basadas en Arch:

~~~ bash
sudo pacman -S vim neovim
~~~

para gestionar plugins apartes uso vim-Plug, se instala (para vim) de la siguiente forma:

~~~ bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
~~~
para neovim
~~~ bash
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
~~~

Para configurar nuestro neovim, dentro de la carpeta `.config`, crearemos una carpeta nvim en la cual crearemos nuestro init.vim (es como el .vimrc pero de neovim).

**init.vim**
~~~
syntax on
set title             " Muestra el nombre del archivo en la ventana de la terminal
set number            " Muestra los números de las líneas
set relativenumber
set mouse=a           " Permite la integración del mouse (seleccionar texto, mover el cursor)
set clipboard+=unnamedplus

set cursorline        " Resalta la línea actual
" set colorcolumn=120  " Muestra la columna límite a 120 caracteres

" Indentación a 2 espacios
set tabstop=2
set shiftwidth=2
set softtabstop=2
set shiftround
set expandtab         " Insertar espacios en lugar de <Tab>s

set hidden            " Permitir cambiar de buffers sin tener que guardarlos

set ignorecase        " Ignorar mayúsculas al hacer una búsqueda
set smartcase         " No ignorar mayúsculas si la palabra a buscar contiene mayúsculas

set spelllang=en,es   " Corregir palabras usando diccionarios en inglés y español
set showcmd         

" ARCHIVOS DE CONFIGURACION EXTERNOS

so ~/.config/nvim/keymaps.vim
so ~/.config/nvim/plugs.vim
so ~/.config/nvim/plugsConfig.vim

" CONFIGURACION DEL TEMA
set termguicolors    " Activa true colors en la terminal
set background=dark   " Fondo del tema: light o dark
colorscheme onedark

set laststatus=2
~~~

como podemos observar dentro de este archivo llamamos a otros tres archivos que son:
- keymap _(donde configuro mis atajos de teclado)_
- plugs _(donde esta la lista de plugins)_
- plugsConfig _(donde esta la configuracion de cada plugin)_

Configuracion (del 15/08/22) de estos archivos
~~~ bash
let g:mapleader = ' '  " Definir espacio como la tecla líder

nnoremap <leader>w :w<CR>           " Guardar con <líder> + s

" nnoremap <leuder>e :e $MYVIMRC<CR>  " Abrir el archivo init.vim con <líder> + e

" Usar <líder> + y para copiar al portapapeles
vnoremap <leader>y "+y
nnoremap <leader>y "+y

" Usar <líder> + d para cortar al portapapeles
vnoremap <leader>d "+d
nnoremap <leader>d "+d

" Usar <líder> + p para pegar desde el portapapeles
nnoremap <leader>p "+p
vnoremap <leader>p "+p
nnoremap <leader>P "+P
vnoremap <leader>P "+P

" Moverse al buffer siguiente con <líder> + l
nnoremap <leader>l :bnext<CR>

" Moverse al buffer anterior con <líder> + j
nnoremap <leader>j :bprevious<CR>

" Cerrar el buffer actual con <líder> + q
nnoremap <leader>q :q<CR>

" redimencionar diviciones
nnoremap <Leader>> 10<C-w>>
nnoremap <Leader>< 10<C-w><

" NERDTree
nnoremap <Leader>f :NERDTreeFind<CR>

" Easymotion
nmap <Leader>s <Plug>(easymotion-s2)
~~~

~~~ bash
call plug#begin()

" Themes
Plug 'https://github.com/joshdick/onedark.vim.git'

" Status Bar
Plug 'itchyny/lightline.vim'

" Tree
Plug 'scrooloose/nerdtree'

" Syntax
Plug 'sheerun/vim-polyglot'
Plug 'norcalli/nvim-colorizer.lua'     " coloretes en css

" IDE
Plug 'easymotion/vim-easymotion' " esay move in a file
"Plug 'davidhalter/jedi-vim'   " jedi for python -> autocomplete
"Plug 'zchee/deoplete-jedi' " Python autocompletion
"Plug 'davidhalter/jedi-vim' " Just to add the python go-to-definition and similar features, autocompletion
Plug 'sheerun/vim-polyglot' 
Plug 'Townk/vim-autoclose' " Automatically close parenthesis, etc
Plug 'Shougo/context_filetype.vim' " Completion from other opened files



call plug#end()
~~~

~~~ bash
"Configuracion de los plugs

" ----------------------------------------------------------------------------------------------------------------------
" configuracion de one dark
"Use 24-bit (true-color) mode in Vim/Neovim when outside tmux.
"If you're using tmux version 2.2 or later, you can remove the outermost $TMUX check and use tmux's 24-bit color support
"(see < http://sunaku.github.io/tmux-24bit-color.html#usage > for more information.)
if (empty($TMUX))
  if (has("nvim"))
    "For Neovim 0.1.3 and 0.1.4 < https://github.com/neovim/neovim/pull/2198 >
    let $NVIM_TUI_ENABLE_TRUE_COLOR=1
  endif
  "For Neovim > 0.1.5 and Vim > patch 7.4.1799 < https://github.com/vim/vim/commit/61be73bb0f965a895bfb064ea3e55476ac175162 >
  "Based on Vim patch 7.4.1770 (`guicolors` option) < https://github.com/vim/vim/commit/8a633e3427b47286869aa4b96f2bfc1fe65b25cd >
  " < https://github.com/neovim/neovim/wiki/Following-HEAD#20160511 >
  if (has("termguicolors"))
    set termguicolors
  endif
endif

" ----------------------------------------------------------------------------------------------------------------------
" LIGHTLINE
let g:lightline = {
  \ 'colorscheme': 'onedark',
  \ }

" ----------------------------------------------------------------------------------------------------------------------
"  NERDTREE {{{
let NERDTreeShowHidden=1
let NERDTreeQuitOnOpen=1
let NERDTreeAutoDeleteBuffer=1
let NERDTreeMinimalUI=1
let NERDTreeDirArrows=1
let NERDTreeShowLineNumbers=1
let NERDTreeMapOpenInTab='\t'
" }}}
" jedi options {{{ 
let g:jedi#auto_initialization = 1
let g:jedi#completions_enabled = 0
let g:jedi#auto_vim_configuration = 0
let g:jedi#smart_auto_mappings = 0
let g:jedi#popup_on_dot = 0
let g:jedi#completions_command = ""
let g:jedi#show_call_signatures = "1"
let g:jedi#show_call_signatures_delay = 0
let g:jedi#use_tabs_not_buffers = 0
let g:jedi#show_call_signatures_modes = 'i'  " ni = also in normal mode
let g:jedi#enable_speed_debugging=0
let g:jedi#force_py_version = 3
let g:pymode_python = 'python3'

" }}}
~~~

para instalar los plugins basta con usar `:PlugInstall` o `:PlugUpdate` desde la linea de comando de vim.

> muchas gracias a nicolas schurmann por servir de base para esta configuracion sencilla
