*1.- Descarga ISO Parrot*:
https://www.parrotsec.org/download/

*2.- Despues de la intalación: *
```linux
parrot-upgrade
```

Las tripas de parrot-upgrade
![[Pasted image 20240427230815.png]]

Si a la hora de upgrade te sale algo del estilo:
![[Pasted image 20240427235323.png]]
Mejor ejecutar asi y ver lo que te borraría, así puedes ir borrandolos uno a uno, mejor:
![[Pasted image 20240427235453.png]]

*3.-  Instación Bspwm / Sxhkd:*
bspwm -> bspwmrc (fichero de configuración)
sxhkd -> sxhkdrc (fichero de configuración) 

En resumen, primeramente ejecutaremos el siguiente comando:

- **apt install build-essential git vim xcb libxcb-util0-dev libxcb-ewmh-dev libxcb-randr0-dev libxcb-icccm4-dev libxcb-keysyms1-dev libxcb-xinerama0-dev libasound2-dev libxcb-xtest0-dev libxcb-shape0-dev**

Posteriormente, aplicaremos una actualización del sistema con el comando ‘**apt update**‘. Acto seguido, tenéis que dirigiros a la carpeta de descargas de vuestro equipo y descargar los proyectos ‘**bswpm**‘ y ‘**sxhkd**‘ con los siguientes comandos:

- git clone https://github.com/baskerville/bspwm.git
- git clone https://github.com/baskerville/sxhkd.git

Para instalar cada uno de estos, lo que debéis hacer es meteros en ambos directorios por separado y ejecutar los comandos ‘**make**‘ y ‘**sudo make install**‘.

Los directorios de configuración no existen por lo que habrá que crearlos:
![[Pasted image 20240428001411.png]]

Después iremos:
![[Pasted image 20240428001938.png]]

Moveremos estos archivos de configuración a los directorios que hemos creado:

![[Pasted image 20240428002221.png]]

![[Pasted image 20240428002634.png]]
Ahora instalaremos la terminal kity (por defecto abrimos con gnome):
```
apt intall kitty
```

En el archivo sxhkdrc-> Iremos configurando los atajos:

![[Pasted image 20240428003747.png]]
![[Pasted image 20240428003716.png]]
![[Pasted image 20240428004231.png]]
![[Pasted image 20240428004509.png]]
![[Pasted image 20240428004643.png]]
![[Pasted image 20240428005251.png]]
![[Pasted image 20240428013215.png]]
A continuación tenéis el enlace al archivo de configuración ‘**bspwm_resize**‘ que usamos al final de esta clase:

```bash
#!/usr/bin/env dash

if bspc query -N -n focused.floating > /dev/null; then
	step=20
else
	step=100
fi

case "$1" in
	west) dir=right; falldir=left; x="-$step"; y=0;;
	east) dir=right; falldir=left; x="$step"; y=0;;
	north) dir=top; falldir=bottom; x=0; y="-$step";;
	south) dir=top; falldir=bottom; x=0; y="$step";;
esac

bspc node -z "$dir" "$x" "$y" || bspc node -z "$falldir" "$x" "$y"
```

**Bspwm (Binary Space Partitioning Window Manager)**

Bspwm es un gestor de ventanas que utiliza la técnica de partición binaria del espacio para organizar las ventanas en el escritorio. Es conocido por su simplicidad y eficiencia, ya que se configura y se controla exclusivamente a través de scripts y comandos en la terminal. Bspwm no maneja teclados ni otros dispositivos de entrada por sí mismo, sino que delega esta tarea a otras herramientas, lo que permite una mayor personalización y flexibilidad.

Cada ventana se organiza automáticamente de manera que ocupe un área divisoria del espacio disponible en el escritorio, optimizando el uso del espacio y facilitando la navegación entre diferentes aplicaciones y documentos abiertos.

**Sxhkd (Simple X Hotkey Daemon)**

Sxhkd es un demonio de teclas de acceso rápido para sistemas X Window. Funciona en conjunto con gestores de ventanas como Bspwm y permite a los usuarios asignar acciones a combinaciones de teclas y botones del mouse. Su configuración se realiza a través de un archivo de texto plano, donde el usuario define las combinaciones de teclas y las acciones correspondientes que se deben ejecutar. Sxhkd es altamente configurable y ligero, diseñado para ser rápido y eficiente en el manejo de eventos de entrada, lo que lo hace ideal para entornos donde los recursos del sistema son limitados o cuando se busca una experiencia de usuario altamente personalizable y controlada.

Ambos programas son muy populares en la comunidad de entusiastas de Linux que prefieren un entorno de escritorio altamente personalizable y orientado al uso de teclado.

*3.- Instalación de la Polybar / Picom y Rofi*

En esta clase, instalamos Polybar, Picom y Rofi, paquetes esenciales para personalizar tu entorno Bspwm, mejorando la interfaz y la usabilidad.

- **Polybar**: Es una barra de tareas altamente personalizable para sistemas de ventanas X. Polybar se destaca por su flexibilidad y capacidad para mostrar información variada como la fecha, la utilización del CPU, la memoria, y mucho más. Puedes configurar completamente su apariencia y los módulos que muestra, lo que la hace muy popular entre los usuarios que desean un escritorio minimalista y funcional.
- **Picom**: Es un compositor para el sistema de ventanas X, lo que significa que maneja cómo se muestran las ventanas y los efectos visuales en el escritorio, como sombras, transparencias y animaciones suaves. Picom ayuda a mejorar la estética general del escritorio y reduce el desgarro de la pantalla durante la reproducción de video y el movimiento de ventanas.
- **Rofi**: Es un lanzador de aplicaciones ligero y personalizable, que también puede servir como conmutador de ventanas y más. Rofi permite a los usuarios buscar y lanzar aplicaciones rápidamente, cambiar entre ventanas activas, o incluso ejecutar comandos personalizados. Su interfaz es altamente configurable, lo que permite a los usuarios adaptarla a sus necesidades específicas y estética del escritorio.

Iremos al repositorio de github para descargar picom:
https://github.com/yshui/picom

Primero tendremos que instalar estos paquetes (Dependencias) antes de clonar el *picom*:
```linux
apt install libconfig-dev libdbus-1-dev libegl-dev libev-dev libgl-dev libepoxy-dev libpcre2-dev libpixman-1-dev libx11-xcb-dev libxcb1-dev libxcb-composite0-dev libxcb-damage0-dev libxcb-dpms0-dev libxcb-glx0-dev libxcb-image0-dev libxcb-present-dev libxcb-randr0-dev libxcb-render0-dev libxcb-render-util0-dev libxcb-shape0-dev libxcb-util-dev libxcb-xfixes0-dev libxext-dev meson ninja-build uthash-dev -y
```

y luego 
```linux
apt update
```

Clonaremos el repositorio en la carpeta /Descargas como siempre:
```linux
git clone https://github.com/yshui/picom
```

Luego volveremos al repositorio github y seguiremos las instrucciones de To build(dentro de la carpeta picom de Descargas):
```linux
[k4rma@parrot]─[~/Descargas]
└──╼ $cd picom
┌─[k4rma@parrot]─[~/Descargas/picom]
└──╼ $which meson 
/usr/bin/meson
┌─[k4rma@parrot]─[~/Descargas/picom]
└──╼ $meson setup --buildtype=release build
```

```linux
k4rma@parrot]─[~/Descargas/picom]                                                                
└──╼ $ninja -C build
ninja: Entering directory `build'
[41/41] Linking target src/picom
┌─[k4rma@parrot]─[~/Descargas/picom]
└──╼ $ninja -C build install
```

Ya tendriamos picom:
![[Pasted image 20240428190751.png]]

Ahora iremos con *rofi*:
```linux
sudo apt install rofi
```

Para mostrar rufi:
```linux
rofi -show run
```

![[Pasted image 20240428191114.png]]

```linux
[k4rma@parrot]─[~/Descargas/picom]
└──╼ $cd ~/.config/sxhkd
┌─[k4rma@parrot]─[~/.config/sxhkd]
└──╼ $nano sxhkdrc 
```

En sxhkdrc:
![[Pasted image 20240428191800.png]]

Pantalla de bloqueo para ver si funciona en entorno de bspwm:
```linux
kill -9 -1
```

Si al reiniciar no está la opción de abrir la bspwn:
```linux
sudo nano /usr/share/xsessions/bspwm.desktop
```

```linux
[Desktop Entry]
Name=BSPWM
Comment=Tiling Window Manager
Exec=/usr/local/bin/bspwm
Type=XSession
```

```linux
sudo systemctl restart display-manager
```


*4.- Configurando fuentes, kittu e instalación Feh*

Para crear el atajo para firefox:
![[Pasted image 20240504105940.png]]

![[Pasted image 20240504105838.png]]

Para recargar y que surta efecto en el sxhkd:
Windows + esc

https://www.nerdfonts.com/font-downloads -> HackNerd Fonts

![[Pasted image 20240504105347.png]]
![[Pasted image 20240504110756.png]]

Listaremos el contenido del zip:
![[Pasted image 20240504110852.png]]

![[Pasted image 20240504111105.png]]

Descargaremos el zip:
![[Pasted image 20240504111159.png]]

Borraremos los archivos que no necesitamos:
![[Pasted image 20240504111348.png]]

Procederemos a instalar la zsh:
![[Pasted image 20240504111514.png]]

Habilitaremos la clipboard bidireccional:
Primero saldremos de root para configurar la clipboard;
![[Pasted image 20240504111711.png]]

![[Pasted image 20240504111922.png]]
Añadiremos la archivo esto para que se ejecute en segundo plano(&)
![[Pasted image 20240504112048.png]]

Ahora iremos a por la kitty:
![[Pasted image 20240504113834.png]]

https://github.com/kovidgoyal/kitty

Descargar -> Descargas
![[Pasted image 20240504113922.png]]

Borraremos la versión anterior:
![[Pasted image 20240504115119.png]]

![[Pasted image 20240504115313.png]]

![[Pasted image 20240504115350.png]]

Borraremos el comprimido, después de haberlo descargado:
![[Pasted image 20240504115453.png]]

Miraremos si el achivo te crea un directori kitty en local o no:
![[Pasted image 20240504115708.png]]

No te crea directorio, sino que te lo descarga en global:
![[Pasted image 20240504115746.png]]

Crearemos una carpeta y moveremos a ese directorio:
![[Pasted image 20240504115954.png]]
![[Pasted image 20240505105425.png]]
![[Pasted image 20240505105507.png]]
![[Pasted image 20240505105613.png]]

![[Pasted image 20240505105657.png]]

![[Pasted image 20240505105849.png]]

![[Pasted image 20240505111339.png]]

Y en archivo kitty.conf le metemos el archivo que está más abajo.

windows+esc -> actualizar cambios

Encontrar (-E)multiples matches con root o K4rma:
![[Pasted image 20240505150848.png]]

Para ver documentación de la kitty -> https://sw.kovidgoyal.net/kitty/

Creamos archivo color.ini dentro de la carpeta kitty y pegamos la configuración que aparece abajo 
![[Pasted image 20240505153019.png|450]]

Ya tendremos el parrot con la nueva configuracion y el zsh:
![[Pasted image 20240505172117.png]]

Para ver imagenes desde pantalla instalaremos este paquete:
![[Pasted image 20240505172514.png]]

![[Pasted image 20240505172719.png]]

Ahora instalaremos feh(Para configurar un fondo de pantalla) :
![[Pasted image 20240505172807.png]]

Despues descargaremos la imagen en el directorio descargas y no dirigiremos a :
![[Pasted image 20240505174349.png]]
![[Pasted image 20240505174642.png]]
![[Pasted image 20240505174700.png]]
![[Pasted image 20240505174941.png]]

![[Pasted image 20240505175438.png]]
![[Pasted image 20240505175411.png]]

Ahora aplicaremos la configuración creada en el usuario k4rma a root también:
![[Pasted image 20240505180926.png]]
De esta forma se aplicarán los cambios tanto para root como para k4rma

- **Kitty:** Kitty es un emulador de terminal moderno para sistemas operativos basados en Unix, como Linux y macOS. Es especialmente conocido por su eficiencia y capacidad para manejar gráficos modernos como imágenes y emojis directamente en la terminal. Kitty es altamente personalizable y ofrece funcionalidades avanzadas como pestañas, división de ventanas, y transparencia, entre otros. Utiliza la aceleración por GPU para renderizar los textos, lo que lo hace particularmente rápido.
- **Hack Nerd Fonts:** Es una versión modificada de la fuente mono-espaciada Hack, que ha sido ampliada con una gran cantidad de iconos y símbolos adicionales, conocidos como “glyphs”. Estos incluyen íconos de populares herramientas de desarrollo y sistemas, lo que hace a esta fuente muy útil para desarrolladores y usuarios de terminales, ya que permite visualizar íconos específicos de herramientas directamente en la interfaz de la terminal.
- **Feh:** Feh es un visor de imágenes ligero y rápido para Linux que también puede ser utilizado para configurar fondos de pantalla en sistemas de escritorio. Es muy eficiente y funciona bien en entornos de escritorio ligeros o configuraciones minimalistas, ya que no depende de grandes librerías gráficas. Feh puede ser utilizado en scripts y configuraciones para cambiar automáticamente fondos de pantalla o para mostrar imágenes en presentaciones de diapositivas.

Estas herramientas son bastante populares en la comunidad de usuarios avanzados de Linux, especialmente aquellos que prefieren entornos altamente personalizables y eficientes.

Por aquí tienes el archivo ‘**kitty.conf**‘ para la Kitty:

```
enable_audio_bell no

include color.ini

font_family HackNerdFont
font_size 13

disable_ligatures never

url_color #61afef

url_style curly

map ctrl+left neighboring_window left
map ctrl+right neighboring_window right
map ctrl+up neighboring_window up
map ctrl+down neighboring_window down

map f1 copy_to_buffer a
map f2 paste_from_buffer a
map f3 copy_to_buffer b
map f4 paste_from_buffer b

cursor_shape beam
cursor_beam_thickness 1.8

mouse_hide_wait 3.0
detect_urls yes

repaint_delay 10
input_delay 3
sync_to_monitor yes

map ctrl+shift+z toggle_layout stack
tab_bar_style powerline

inactive_tab_background #e06c75
active_tab_background #98c379
inactive_tab_foreground #000000
tab_bar_margin_color black

map ctrl+shift+enter new_window_with_cwd
map ctrl+shift+t new_tab_with_cwd

background_opacity 0.95

shell zsh
```

Por aquí tienes el archivo de configuración ‘**color.ini**‘ para la Kitty: 
```
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

enable_audio_bell no
```


Os compartimos a continuación el fondo de pantalla que utilizamos en esta clase:

![[wp7885623-4k-ultra-hd-neon-mask-boy-wallpapers(1).jpg]]

*5.- Despliegue Polybar*
**Polybar** es una herramienta muy utilizada en la personalización de entornos de escritorio en sistemas Linux, especialmente en aquellos que emplean gestores de ventanas livianos o “tiling window managers” como **i3**, **bspwm**, y otros. Es una barra de estado que se destaca por ser altamente configurable y modular.

Polybar permite a los usuarios crear barras de estado que se adaptan precisamente a sus necesidades y estética del escritorio. Puedes configurar elementos como módulos de reloj, indicadores de batería, controles de volumen, monitores de sistema (como CPU, memoria, etc.), espacios de trabajo y muchos otros. Cada módulo en la barra puede ser personalizado en términos de funcionalidad y apariencia.

La configuración de Polybar se realiza a través de archivos de texto plano, lo que proporciona una gran flexibilidad. Los usuarios pueden escribir sus propios módulos usando scripts en diferentes lenguajes de programación o adaptar módulos existentes para personalizar su experiencia. Además, Polybar es capaz de lanzar y mostrar notificaciones o resultados de comandos específicos, lo que lo hace una herramienta extremadamente potente para aquellos que desean tener un control total sobre la información que se muestra en su entorno de escritorio.

En la siguiente clase, trabajaremos en las esquinas, el sombreado y los difuminados.

En la ventana descargas -> ```git clone https://github.com/VaughnValle/blue-sky.git```

![[Pasted image 20240505184046.png]]

![[Pasted image 20240505184154.png]]

![[Pasted image 20240505185216.png]]

Nos pasaremos a root para deplegar la polybar:
![[Pasted image 20240505191552.png]]

Para refrescar la cache:
![[Pasted image 20240505191713.png]]

Ahora recargaremos con el windows+shift+r:

Tenemos la Polybar pero no está muy bien configurada:
![[Pasted image 20240506175116.png]]

*6.- Configuración bordeados, sombras y difuminados*

Por aquí os comparto mi archivo ‘**picom.conf**‘:

```
##############################################################################
#                                  CORNERS                                   #
##############################################################################
# requires: https://github.com/sdhand/compton
corner-radius = 20;
rounded-corners-exclude = [
  #"window_type = 'normal'",
  #"class_g = 'firefox'",
];

round-borders = 20;
round-borders-exclude = [
  #"class_g = 'TelegramDesktop'",
];

# Specify a list of border width rules, in the format `PIXELS:PATTERN`, 
# Note we don't make any guarantee about possible conflicts with the
# border_width set by the window manager.
#
# example:
#    round-borders-rule = [ "2:class_g = 'URxvt'" ];
#
round-borders-rule = [];

##############################################################################
#                                  SHADOWS                                   #
##############################################################################

# Enabled client-side shadows on windows. Note desktop windows 
# (windows with '_NET_WM_WINDOW_TYPE_DESKTOP') never get shadow, 
# unless explicitly requested using the wintypes option.
#
shadow = true

# The blur radius for shadows, in pixels. (defaults to 12)
shadow-radius = 15

# The opacity of shadows. (0.0 - 1.0, defaults to 0.75)
shadow-opacity = .5

# The left offset for shadows, in pixels. (defaults to -15)
shadow-offset-x = -15

# The top offset for shadows, in pixels. (defaults to -15)
shadow-offset-y = -15

# Avoid drawing shadows on dock/panel windows. This option is deprecated,
# you should use the *wintypes* option in your config file instead.
#
# no-dock-shadow = false

# Don't draw shadows on drag-and-drop windows. This option is deprecated, 
# you should use the *wintypes* option in your config file instead.
#
# no-dnd-shadow = false

# Red color value of shadow (0.0 - 1.0, defaults to 0).
# shadow-red = .18

# Green color value of shadow (0.0 - 1.0, defaults to 0).
# shadow-green = .19

# Blue color value of shadow (0.0 - 1.0, defaults to 0).
# shadow-blue = .20

# Do not paint shadows on shaped windows. Note shaped windows 
# here means windows setting its shape through X Shape extension. 
# Those using ARGB background is beyond our control. 
# Deprecated, use 
#   shadow-exclude = 'bounding_shaped'
# or 
#   shadow-exclude = 'bounding_shaped && !rounded_corners'
# instead.
#
# shadow-ignore-shaped = ''

# Specify a list of conditions of windows that should have no shadow.
#
# examples:
#   shadow-exclude = "n:e:Notification";
#
# shadow-exclude = []
shadow-exclude = [
    "class_g = 'firefox' && argb"
];

# Specify a X geometry that describes the region in which shadow should not
# be painted in, such as a dock window region. Use 
#    shadow-exclude-reg = "x10+0+0"
# for example, if the 10 pixels on the bottom of the screen should not have shadows painted on.
#
# shadow-exclude-reg = "" 

# Crop shadow of a window fully on a particular Xinerama screen to the screen.
# xinerama-shadow-crop = false

##############################################################################
#                                  FADING                                    #
##############################################################################

# Fade windows in/out when opening/closing and when opacity changes,
#  unless no-fading-openclose is used.
#fading = true

# Opacity change between steps while fading in. (0.01 - 1.0, defaults to 0.028)
# fade-in-step = 0.028
fade-in-step = 0.01;

# Opacity change between steps while fading out. (0.01 - 1.0, defaults to 0.03)
# fade-out-step = 0.03
fade-out-step = 0.01;

# The time between steps in fade step, in milliseconds. (> 0, defaults to 10)
# fade-delta = 10

# Specify a list of conditions of windows that should not be faded.
# fade-exclude = []

# Do not fade on window open/close.
# no-fading-openclose = false

# Do not fade destroyed ARGB windows with WM frame. Workaround of bugs in Openbox, Fluxbox, etc.
# no-fading-destroyed-argb = false

##############################################################################
#                                   OPACITY                                  #
##############################################################################

# Opacity of inactive windows. (0.1 - 1.0, defaults to 1.0)
inactive-opacity = 1.0

# Opacity of window titlebars and borders. (0.1 - 1.0, disabled by default)
frame-opacity = 1.0

# Default opacity for dropdown menus and popup menus. (0.0 - 1.0, defaults to 1.0)
opacity = 1.0

# Let inactive opacity set by -i override the '_NET_WM_OPACITY' values of windows.
# inactive-opacity-override = true
inactive-opacity-override = false;

# Default opacity for active windows. (0.0 - 1.0, defaults to 1.0)
active-opacity = 1.0

# Dim inactive windows. (0.0 - 1.0, defaults to 0.0)
# inactive-dim = 0.0

# Specify a list of conditions of windows that should always be considered focused.
# focus-exclude = []
focus-exclude = [ "class_g = 'Cairo-clock'" ];

# Use fixed inactive dim value, instead of adjusting according to window opacity.
# inactive-dim-fixed = 1.0

# Specify a list of opacity rules, in the format `PERCENT:PATTERN`, 
# like `50:name *= "Firefox"`. picom-trans is recommended over this. 
# Note we don't make any guarantee about possible conflicts with other 
# programs that set '_NET_WM_WINDOW_OPACITY' on frame or client windows.
# example:
#    opacity-rule = [ "80:class_g = 'URxvt'" ];
#
# opacity-rule = []

# opacity-rule = [ "98:class_g = 'Polybar'" ]

##############################################################################
#                                    BLUR                                    #
##############################################################################

# Parameters for background blurring, see the *BLUR* section for more information.
blur-method = "dual_kawase"
blur-size = 2
blur-strength = 3

# Blur background of semi-transparent / ARGB windows. 
# Bad in performance, with driver-dependent behavior. 
# The name of the switch may change without prior notifications.
#
blur-background = true

# Blur background of windows when the window frame is not opaque. 
# Implies:
#    blur-background 
# Bad in performance, with driver-dependent behavior. The name may change.
#
#blur-background-frame = false


# Use fixed blur strength rather than adjusting according to window opacity.
#blur-background-fixed = false


# Specify the blur convolution kernel, with the following format:
# example:
#   blur-kern = "5,5,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1";
#
# blur-kern = ''
# blur-kern = "3x3box";

# Exclude conditions for background blur.
# blur-background-exclude = []
#blur-background-exclude = [
#    "! name~=''",
#    "name *= 'slop'",
#    "window_type = 'dock'",
#    "window_type = 'desktop'",
#    "_GTK_FRAME_EXTENTS@:c"
#];

##############################################################################
#                                    GENERAL                                 #
##############################################################################

# Daemonize process. Fork to background after initialization. Causes issues with certain (badly-written) drivers.
# daemon = false

# Specify the backend to use: `xrender`, `glx`, or `xr_glx_hybrid`.
# `xrender` is the default one.
#
# backend = 'glx'
backend = "glx";

# Enable/disable VSync.
# vsync = false
vsync = false

# Enable remote control via D-Bus. See the *D-BUS API* section below for more details.
# dbus = false

# Try to detect WM windows (a non-override-redirect window with no 
# child that has 'WM_STATE') and mark them as active.
#
# mark-wmwin-focused = false
mark-wmwin-focused = true;

# Mark override-redirect windows that doesn't have a child window with 'WM_STATE' focused.
# mark-ovredir-focused = false
mark-ovredir-focused = true;

# Try to detect windows with rounded corners and don't consider them 
# shaped windows. The accuracy is not very high, unfortunately.
#
# detect-rounded-corners = false
detect-rounded-corners = true;

# Detect '_NET_WM_OPACITY' on client windows, useful for window managers
# not passing '_NET_WM_OPACITY' of client windows to frame windows.
#
# detect-client-opacity = false
detect-client-opacity = true;

# Specify refresh rate of the screen. If not specified or 0, picom will 
# try detecting this with X RandR extension.
#
# refresh-rate = 60
refresh-rate = 0

# Limit picom to repaint at most once every 1 / 'refresh_rate' second to 
# boost performance. This should not be used with 
#   vsync drm/opengl/opengl-oml
# as they essentially does sw-opti's job already, 
# unless you wish to specify a lower refresh rate than the actual value.
#
# sw-opti = 

# Use EWMH '_NET_ACTIVE_WINDOW' to determine currently focused window, 
# rather than listening to 'FocusIn'/'FocusOut' event. Might have more accuracy, 
# provided that the WM supports it.
#
# use-ewmh-active-win = false

# Unredirect all windows if a full-screen opaque window is detected, 
# to maximize performance for full-screen windows. Known to cause flickering 
# when redirecting/unredirecting windows.
#
# unredir-if-possible = false

# Delay before unredirecting the window, in milliseconds. Defaults to 0.
# unredir-if-possible-delay = 0

# Conditions of windows that shouldn't be considered full-screen for unredirecting screen.
# unredir-if-possible-exclude = []

# Use 'WM_TRANSIENT_FOR' to group windows, and consider windows 
# in the same group focused at the same time.
#
# detect-transient = false
detect-transient = true

# Use 'WM_CLIENT_LEADER' to group windows, and consider windows in the same 
# group focused at the same time. 'WM_TRANSIENT_FOR' has higher priority if 
# detect-transient is enabled, too.
#
# detect-client-leader = false
detect-client-leader = true

# Resize damaged region by a specific number of pixels. 
# A positive value enlarges it while a negative one shrinks it. 
# If the value is positive, those additional pixels will not be actually painted 
# to screen, only used in blur calculation, and such. (Due to technical limitations, 
# with use-damage, those pixels will still be incorrectly painted to screen.) 
# Primarily used to fix the line corruption issues of blur, 
# in which case you should use the blur radius value here 
# (e.g. with a 3x3 kernel, you should use `--resize-damage 1`, 
# with a 5x5 one you use `--resize-damage 2`, and so on). 
# May or may not work with *--glx-no-stencil*. Shrinking doesn't function correctly.
#
# resize-damage = 1

# Specify a list of conditions of windows that should be painted with inverted color. 
# Resource-hogging, and is not well tested.
#
# invert-color-include = []

# GLX backend: Avoid using stencil buffer, useful if you don't have a stencil buffer. 
# Might cause incorrect opacity when rendering transparent content (but never 
# practically happened) and may not work with blur-background. 
# My tests show a 15% performance boost. Recommended.
#
# glx-no-stencil = false

# GLX backend: Avoid rebinding pixmap on window damage. 
# Probably could improve performance on rapid window content changes, 
# but is known to break things on some drivers (LLVMpipe, xf86-video-intel, etc.).
# Recommended if it works.
#
# glx-no-rebind-pixmap = false

# Disable the use of damage information. 
# This cause the whole screen to be redrawn everytime, instead of the part of the screen
# has actually changed. Potentially degrades the performance, but might fix some artifacts.
# The opposing option is use-damage
#
# no-use-damage = false
use-damage = false

# Use X Sync fence to sync clients' draw calls, to make sure all draw 
# calls are finished before picom starts drawing. Needed on nvidia-drivers 
# with GLX backend for some users.
#
# xrender-sync-fence = false

# GLX backend: Use specified GLSL fragment shader for rendering window contents. 
# See `compton-default-fshader-win.glsl` and `compton-fake-transparency-fshader-win.glsl` 
# in the source tree for examples.
#
# glx-fshader-win = ''

# Force all windows to be painted with blending. Useful if you 
# have a glx-fshader-win that could turn opaque pixels transparent.
#
# force-win-blend = false

# Do not use EWMH to detect fullscreen windows. 
# Reverts to checking if a window is fullscreen based only on its size and coordinates.
#
# no-ewmh-fullscreen = false

# Dimming bright windows so their brightness doesn't exceed this set value. 
# Brightness of a window is estimated by averaging all pixels in the window, 
# so this could comes with a performance hit. 
# Setting this to 1.0 disables this behaviour. Requires --use-damage to be disabled. (default: 1.0)
#
# max-brightness = 1.0

# Make transparent windows clip other windows like non-transparent windows do,
# instead of blending on top of them.
#
# transparent-clipping = false

# Set the log level. Possible values are:
#  "trace", "debug", "info", "warn", "error"
# in increasing level of importance. Case doesn't matter. 
# If using the "TRACE" log level, it's better to log into a file 
# using *--log-file*, since it can generate a huge stream of logs.
#
# log-level = "debug"
log-level = "warn";

# Set the log file.
# If *--log-file* is never specified, logs will be written to stderr. 
# Otherwise, logs will to written to the given file, though some of the early 
# logs might still be written to the stderr. 
# When setting this option from the config file, it is recommended to use an absolute path.
#
# log-file = '/path/to/your/log/file'

# Show all X errors (for debugging)
# show-all-xerrors = false

# Write process ID to a file.
# write-pid-path = '/path/to/your/log/file'

# Window type settings
# 
# 'WINDOW_TYPE' is one of the 15 window types defined in EWMH standard: 
#     "unknown", "desktop", "dock", "toolbar", "menu", "utility", 
#     "splash", "dialog", "normal", "dropdown_menu", "popup_menu", 
#     "tooltip", "notification", "combo", and "dnd".
# 
# Following per window-type options are available: ::
# 
#   fade, shadow:::
#     Controls window-type-specific shadow and fade settings.
# 
#   opacity:::
#     Controls default opacity of the window type.
# 
#   focus:::
#     Controls whether the window of this type is to be always considered focused. 
#     (By default, all window types except "normal" and "dialog" has this on.)
# 
#   full-shadow:::
#     Controls whether shadow is drawn under the parts of the window that you 
#     normally won't be able to see. Useful when the window has parts of it 
#     transparent, and you want shadows in those areas.
# 
#   redir-ignore:::
#     Controls whether this type of windows should cause screen to become 
#     redirected again after been unredirected. If you have unredir-if-possible
#     set, and doesn't want certain window to cause unnecessary screen redirection, 
#     you can set this to `true`.
#
wintypes:
{
  tooltip = { fade = true; shadow = true; shadow-radius = 0; shadow-opacity = 1.0; shadow-offset-x = -20; shadow-offset-y = -20; opacity = 0.8; full-shadow = true; }; 
  dnd = { shadow = false; }
  dropdown_menu = { shadow = false; };
  popup_menu    = { shadow = false; };
  utility       = { shadow = false; };
}
```

**Picom** es un compositor para sistemas de ventanas X, utilizado comúnmente en entornos de escritorio Linux. Es un derivado de Compton, que a su vez se basó en xcompmgr-dana, y es ampliamente usado en configuraciones con gestores de ventanas que no incluyen su propio sistema de composición, especialmente en entornos minimalistas como i3, bspwm, y otros.

Picom se encarga de añadir efectos visuales que no solo mejoran la estética del escritorio, sino que también pueden ayudar a hacer la interfaz más amigable y menos cansada para la vista.

Entre las funcionalidades que ofrece Picom se encuentran:

- **Sombras**: Añade sombras a las ventanas, lo que ayuda a mejorar la percepción de profundidad en el escritorio. Las sombras pueden ser configuradas en términos de color, tamaño, desenfoque, y opacidad.
- **Bordeados y esquinas redondeadas**: Permite redondear las esquinas de las ventanas, lo que suaviza la apariencia general del entorno de escritorio. También puede gestionar los bordes de las ventanas.
- **Transparencias y difuminados**: Picom puede gestionar la transparencia de las ventanas, los menús y la terminal, permitiendo configurar diferentes niveles de transparencia y efectos de desenfoque. Estos efectos de desenfoque se pueden aplicar a áreas detrás de ventanas transparentes para mejorar la legibilidad y la estética.
- **Animaciones**: Aunque más limitado en comparación con otros compositores como Compiz, Picom también puede manejar algunas animaciones básicas para minimizar y restaurar ventanas.
- **Prevención de tearing**: Picom ayuda a eliminar o reducir el “tearing” de la pantalla, un problema común donde la imagen no se sincroniza correctamente durante el refresco de la pantalla, lo que resulta en una línea horizontal o varias que descomponen la imagen correctamente renderizada.

La configuración de Picom se realiza a través de un archivo de configuración, donde los usuarios pueden detallar sus preferencias para cada uno de estos efectos. Esto lo hace altamente personalizable y muy popular entre los usuarios que buscan mejorar tanto el rendimiento como la apariencia de sus escritorios.

Recordad que este archivo es totalmente personalizable y debéis ajustarlo en base a las capacidades de vuestro equipo o máquina virtual, para evitar que os genere impacto en el rendimiento.

![[Pasted image 20240506175251.png| 1200]]

![[Pasted image 20240506180024.png]]
Ahora configuraremos en el archivo bspwmrc para que corra como demonio:
![[Pasted image 20240506180143.png]]
Windows+Shift+R

![[Pasted image 20240506225250.png]]

En el archivo añadir:
![[Pasted image 20240506225202.png]]

*7.-Configuración ZSH e instalación Powerlevel10k*

**ZSH (Z Shell)**

ZSH, o Z Shell, es un intérprete de comandos para sistemas Unix, que se utiliza como una alternativa avanzada al tradicional shell Bash. Ofrece muchas mejoras en términos de características, plugins, y temas, lo que lo hace muy popular entre los usuarios avanzados y desarrolladores.

Algunas de las características más notables de ZSH incluyen:

- **Autocompletado**: ZSH proporciona un sistema de autocompletado potente y versátil que puede predecir comandos basados en el contexto, incluyendo opciones y parámetros.
- **Corrección de errores**: Ofrece sugerencias de corrección cuando se escribe un comando erróneamente.
- **Gestión de scripts**: ZSH es compatible con todos los scripts de Bourne Shell y Bash, y añade sus propias mejoras en la programación de scripts.
- **Personalización**: Se puede personalizar profundamente mediante temas y configuraciones que pueden cambiar su apariencia y comportamiento.

**Powerlevel10k**

Powerlevel10k es un tema para ZSH diseñado para ser visualmente atractivo y altamente informativo. Está diseñado para ser una versión más rápida y personalizable del popular tema Powerlevel9k.

Algunas de las características que lo hacen destacar incluyen:

- **Velocidad**: Es significativamente más rápido que otros temas para ZSH, lo que reduce el tiempo de inicio del terminal y la latencia al escribir comandos.
- **Personalizable**: Viene con un asistente de configuración que guía a los usuarios a través del proceso de configuración, permitiendo personalizar el prompt dependiendo de las preferencias del usuario.
- **Información contextual**: Muestra información útil en el prompt según el contexto, como el estado del repositorio git, el usuario actual, el tiempo de ejecución de comandos, y mucho más.
- **Iconos y fuentes**: Utiliza fuentes de Nerd Fonts para mostrar iconos y otros elementos gráficos que hacen que la información sea fácil de leer y visualmente atractiva.

Juntos, ZSH y Powerlevel10k ofrecen una experiencia de terminal rica y eficiente, mejorando tanto la funcionalidad como la apariencia del entorno de línea de comandos. Estas herramientas son particularmente populares entre los desarrolladores y los entusiastas de la personalización de sistemas Unix/Linux.



![[Pasted image 20240507163124.png]]
![[Pasted image 20240507163444.png]]

No utilizaremos estos estilos de la zsh -> https://github.com/romkatv/powerlevel10k

cambiaremos la shell por defecto de root para poder trabajar con root/user en la misma terminal.
![[Pasted image 20240507164555.png]]
![[Pasted image 20240507164523.png]]

![[Pasted image 20240507164715.png]]

```
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc
```

![[Pasted image 20240507165357.png]]

Cambiaremos a la ruta absoluta para no tener problemas al arrancar:
![[Pasted image 20240507165608.png]]

![[Pasted image 20240507165650.png]]
Ahora configuraremos la powerlevel10k a nuestro gusto

Para quitar marca del lado más a la derecha:
![[Pasted image 20240507170123.png]]
comentaremos todos los plugins dentro "#" y añadiremos:
![[Pasted image 20240507171040.png]]

![[Pasted image 20240507170945.png]]

Ahora instalaremos la powerlevel10k para root, la instalación será exactamente igual a user.
![[Pasted image 20240507171907.png]]
Modificaremos el archivo igual que user pero con una salvedad:
En  nano buscar:
![[Pasted image 20240507174135.png]]
borraremos:
![[Pasted image 20240507174319.png]]

Y cambiaremos:
https://www.nerdfonts.com/cheat-sheet -> fire y el icono lo cambiaremos:
![[Pasted image 20240507174603.png]]

Por último tanto en root como en user haremos lo siguiente en el archivo .p10k.zsh:
Buscar:
![[Pasted image 20240507175329.png]]
![[Pasted image 20240507175416.png]]

Para no estar cambiando los cambio en user como en root, vamos a configurar para que todos los cambios que se produzcan en user apunten a root (link simbólico):
![[Pasted image 20240507180302.png]]

![[Pasted image 20240507180333.png]]


(Podría darse el caso de haber conflicto de privilegios en los plugins de zsh)

![[Pasted image 20240507180750.png]]
![[Pasted image 20240507180602.png|800]]

Para el que se ejecute el autocompletado en todos los user y root (zsh-autosuggestions.zsh) y no tener que estar ejecutando:
![[Pasted image 20240507181342.png]]

Haremos los siguiente:
![[Pasted image 20240507181445.png]]

![[Pasted image 20240507182145.png ]]

Ahora instalaremos otro plugin, (esc+esc y te pone sudo):
https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/sudo/sudo.plugin.zsh -> en raw

![[Pasted image 20240511183452.png]]

![[Pasted image 20240511183555.png]]

![[Pasted image 20240511184240.png]]

![[Pasted image 20240511184350.png]]

![[Pasted image 20240511191144.png]]

Para borrar el histórico:
![[Pasted image 20240511191328.png]]

Finalmente para autocompletado con TAB pegaremos este código en el .zshrc
```
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
```


*8.-Instalación de Batcat y Lsd*

**Batcat**

Batcat es la versión distribuida en Debian y Ubuntu del programa **bat**, una herramienta de línea de comandos que sirve como una versión mejorada del clásico comando **cat** de Unix. Batcat ofrece resaltado de sintaxis para una gran variedad de lenguajes de programación y formatos de archivo, facilitando la lectura y comprensión del código directamente en la terminal. Además, integra funcionalidades como la integración con git para mostrar diferencias en el código, numeración de líneas, y la capacidad de previsualizar el contenido de archivos en formato paginado.

Esta herramienta es especialmente útil para desarrolladores y personas que trabajan frecuentemente con código fuente o scripts, ya que mejora significativamente la legibilidad y la eficiencia al explorar o revisar archivos en la terminal.

**Lsd**

Lsd (abreviatura de LSDeluxe) es un reemplazo moderno para el tradicional comando **ls** de Unix, diseñado para mejorar la visualización de los archivos en la terminal. Lsd ofrece un resaltado de colores basado en el tipo de archivo y permisos, además de utilizar iconos para cada tipo de archivo, lo que hace que la navegación y comprensión de la estructura de directorios sea mucho más intuitiva y visualmente agradable. Al igual que bat, lsd soporta fuentes de Nerd Fonts para mostrar iconos, y puede ser completamente personalizado mediante un archivo de configuración para adaptarse a las preferencias del usuario.

Lsd también soporta características como el árbol de directorios directamente en la lista de archivos, lo cual es útil para obtener una visión rápida de la jerarquía de directorios sin necesidad de comandos adicionales. Su funcionalidad y estética mejorada lo hacen popular entre los usuarios que desean una experiencia más rica y eficiente en la línea de comandos.


BAT -> https://github.com/sharkdp/bat/releases/tag/v0.24.0 -> Nos descargaremos el amd64
LSD -> 

-> Nos descargaremos el amd64

![[Pasted image 20240511193659.png]]

![[Pasted image 20240511193732.png]]

Para ver las diferencias:
![[Pasted image 20240511193920.png]]

![[Pasted image 20240511194020.png]]

En lsd está en negrita y para quitarlo , tendremos que quitar todos las lineas con 01;
![[Pasted image 20240511194205.png|1500]]

De está forma te borrará todos los 01 (g para que te lo aplique a todos los matches y no se quedé solo en el primero)
![[Pasted image 20240511194455.png]]

Exportaremos el resultado:
![[Pasted image 20240511194846.png|1500]]

El tema del PATH está ubicado en distintas rutas:
![[Pasted image 20240511195525.png]]

Tanto el LS_COLORS como el PATH lo pasamos al zshrc:
![[Pasted image 20240511195746.png|1500]]

Si tuviese algún problema con abrir burpsuite o alguna app que corre con java:
![[Pasted image 20240511200621.png|1500]]

Podemos intentar con otras instalaciones de java que tenga la máquina:
![[Pasted image 20240511200740.png]]

Si continuase con el problema, leer este artículo y seguir:
https://medium.com/@neat_mahogany_porcupine_191/burpsuite-no-longer-launches-after-parrot-upgrade-d1c6b17cb70d

Si tienes problema con que la app no se centra ni está a pantalla completa, en el zshrc:
![[Pasted image 20240511201114.png]]

En el bspwmrc añadir esta linea:
![[Pasted image 20240511201355.png]]

Para arreglar para cuando lo lanzamos desde la kitty, lo mejor sería configurar un script para que las configuraciones fuesen tanto desde la kitty como desde terminal:
![[Pasted image 20240511202831.png]]
![[Pasted image 20240511203100.png]]

archivo .zshrc
```
# Custom Aliases
# -----------------------------------------------
# bat
alias cat='bat'
alias catn='bat --style=plain'
alias catnp='bat --style=plain --paging=never'

# ls
alias ll='lsd -lh --group-dirs=first'
alias la='lsd -a --group-dirs=first'
alias l='lsd --group-dirs=first'
alias lla='lsd -lha --group-dirs=first'
alias ls='lsd --group-dirs=first'
```

configuracion LS_COLOR
```
export LS_COLORS="rs=0:di=34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.webp=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:"
```


*9.-Configurando la Polybar*

![[Pasted image 20240514150523.png]]

launch.sh -> comentaremos el volumen y hora y día también
![[Pasted image 20240514150306.png]]

![[Pasted image 20240514150654.png]]

current.ini -> ctrl+w (filtrar) ->
![[Pasted image 20240514150751.png]] 

Cambiaremos los siguiente:

![[Pasted image 20240514151106.png]]

ctrl+w
![[Pasted image 20240514151329.png]]

Cambiaremos el icono -> https://www.nerdfonts.com/
![[Pasted image 20240514151616.png|200]]

![[Pasted image 20240514151912.png]]

buscar por -> "font-"
![[Pasted image 20240514152045.png]]

![[Pasted image 20240514152446.png]]

Ahora crearemos nuestro módulos para maquina virtual:

![[Pasted image 20240514152801.png]]

![[Pasted image 20240514152904.png]]

copiaremos el módulo llamado secondary y lo pegaremos llamando ethernet_bar:
![[Pasted image 20240514153218.png]]

![[Pasted image 20240514153308.png]]

![[Pasted image 20240514160346.png]]

![[Pasted image 20240514153851.png]]

Pegaremos este script dentro del archivo para el módulo nuevo:

```
#!/bin/sh
echo "%{F#2495e7}ICONO %{F#ffffff}$(/usr/sbin/ifconfig ens33 | grep "inet " | awk '{print $2}')%{u-}"
```

Donde pone icono sustituiremos por:
![[Pasted image 20240514152641.png]]

la dirección /usr/sbin/ifconfig ens33 existe pero no está contemplado como PATH:
![[Pasted image 20240514154346.png]]

```
nano /home/k4rma/.zshrc
```

![[Pasted image 20240514154657.png]]

Ahora pasaremos a crear el modulo de la máquina víctima (HB sobre todo tun0):

launch.sh
![[Pasted image 20240514172236.png]]

current.ini
![[Pasted image 20240514172734.png]]

*ctr+w +ctr +w para ir iterando por demás busquedas, a la sigiente*

![[Pasted image 20240514173036.png]]

Crearemos el archivo igual que en el caso de ethernet_status
![[Pasted image 20240514173336.png]]

Editaremos el archivo con este scritp, habría que cambiar el icono por el de NerdFonts:
![[Pasted image 20240514175122.png]]
```
#!/bin/sh

IFACE=$(/usr/sbin/ifconfig | grep tun0 | awk '{print $1}' | tr -d ':')

if [ "$IFACE" = "tun0" ]; then
    echo "%{F#1bbf3e}ICONO %{F#ffffff}$(/usr/sbin/ifconfig tun0 | grep "inet " | awk '{print $2}')%{u-}"
else
    echo "%{F#1bbf3e}ICONO %{u-} Disconnected"
fi
```

Por último el botón de la derecha del todo, en current.ini:
![[Pasted image 20240514174925.png]]

Configuraremos el modulo del target:

launch.sh
![[Pasted image 20240514180113.png]]

current.ini
![[Pasted image 20240514180650.png]]

![[Pasted image 20240514180708.png]]

![[Pasted image 20240514181149.png]]
![[Pasted image 20240514201353.png]]

Pegar esto dentro del archivo, cambiando el icono:
```
#!/bin/bash

ip_address=$(/bin/cat /home/s4vitar/.config/bin/target | awk '{print $1}')
machine_name=$(/bin/cat /home/s4vitar/.config/bin/target | awk '{print $2}')

if [ $ip_address ] && [ $machine_name ]; then
    echo "%{F#e51d0b}ICONO %{F#ffffff}$ip_address%{u-} - $machine_name"
else
    echo "%{F#e51d0b}ICONO %{u-}%{F#ffffff} No target"
fi
```

![[Pasted image 20240514181712.png]]
![[Pasted image 20240514181826.png]]
Tendremos que crear el directorio bin
![[Pasted image 20240514182007.png]]
![[Pasted image 20240514182922.png]]

![[Pasted image 20240514194950.png]]

En el archivo de .zshrc añadir esto:

```
# Custom functions
# -------------------------
# Set Victim Target

function settarget(){
    ip_address=$1
    machine_name=$2
    echo "$ip_address $machine_name" > /home/k4rma/.config/bin/target
}
```

```
# Clear Victim Target

function cleartarget(){
	echo '' > /home/s4vitar/.config/bin/target
}
```

![[Pasted image 20240514200401.png]]

Poner colorines a el modulo que te indica en que pantalla me encuentro:
![[Pasted image 20240514203318.png]]

![[Pasted image 20240514203127.png]]

![[Pasted image 20240514203513.png]]

Instaslaremos el fzf -> https://github.com/junegunn/fzf -> Leer el manual
```
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

Ejecutaremos esta instruccion como root y como usuario no privilegiado

cat + ctrl+t -> busqueda de lo que le introduzcas luego.
![[Pasted image 20240514204657.png]]

** + ctr +r
![[Pasted image 20240514204925.png]]

*10.-Configuración e integración de NVchad en Neovim*

**Neovim**

Neovim es un editor de texto extensible basado en Vim, diseñado para mejorar la eficiencia en la edición de código y la experiencia del usuario. Se desarrolló como una bifurcación de Vim con el objetivo de modernizar el código base, introducir mejoras en la arquitectura y facilitar la contribución de la comunidad. Neovim añade características como una API mejorada para plugins y soporte integrado para ejecutar procesos de fondo asíncronos, lo que permite una experiencia más fluida y rica en características sin bloquear la interfaz del usuario mientras se ejecutan tareas complejas.

**NVChad**

NVChad es un framework de configuración para Neovim que ofrece una experiencia de usuario mejorada y una suite de características prediseñadas. Es conocido por su interfaz de usuario estéticamente agradable y por su enfoque en la simplicidad y la eficiencia. NVChad viene preconfigurado con una serie de plugins cuidadosamente seleccionados y configuraciones optimizadas que abordan tanto la funcionalidad como la visualización. Esto incluye soporte para temas de color, gestión de buffers mejorada, autocompletado inteligente, y mucho más, todo ello manteniendo un rendimiento rápido. NVChad está diseñado para ser fácil de instalar y configurar, ofreciendo a los usuarios una plataforma robusta sobre la cual pueden construir o adaptar según sus necesidades específicas.

Ambas herramientas están diseñadas para complementarse entre sí, donde Neovim proporciona el entorno base y NVChad aprovecha y amplía sus capacidades, facilitando una experiencia de usuario altamente personalizada y eficiente para programadores y editores de texto.


Instalaremos en NVchad -> https://nvchad.com/docs/quickstart/install/ 
Pero antes de nada desinstalaremos el Neovim para que no dé conflictos
![[Pasted image 20240514222316.png]]
https://github.com/neovim/neovim/releases/tag/v0.9.5 -> Para instalar la última versión de nvim

![[Pasted image 20240514222959.png]]
![[Pasted image 20240514223019.png]]
![[Pasted image 20240514223224.png]]

Para ver lo que hay dentro:
![[Pasted image 20240514223322.png]]

![[Pasted image 20240514223428.png]]
![[Pasted image 20240514223509.png]]
![[Pasted image 20240514223643.png]]

![[Pasted image 20240514223722.png]]

![[Pasted image 20240514223858.png]]

Quitaremos los $ de cada final de linea (saltos de linea):

![[Pasted image 20240514224333.png]]
Le metemos al principio esta instrucción
```
vim.opt.listchars = "tab:»·,trail:·"
```

![[Pasted image 20240514224530.png]]

Instalaremos esta utilidad para busquedas:
![[Pasted image 20240514224653.png]]

Para que cada vez que hagas un locate te los encuentre rápido
![[Pasted image 20240514224915.png]]

![[Pasted image 20240514225055.png]]

al leer nvim un pytho esta correcto, pero a veces por ejemplo lua da un error:
![[Pasted image 20240514225518.png]]
 Solución:
 ![[Pasted image 20240514225544.png]]
 
E instalaremos:

```
git clone https://github.com/NvChad/starter ~/.config/nvim && nvim
```

Con Nvchad -> muchas utilidades:
esc + espacio th -> temas de neovim
ctr + n -> directorio con archivos para abrir
![[Pasted image 20240514230900.png]]

esc + espacio +f +f -> Para buscar archivos especificos
![[Pasted image 20240514231320.png]]

esc + spacio +f +b -> cuando tienes multiples archivos abiertos para moverte entre los archivos del buffer
:tabnew -> para abrir nuevos espacios de trabajo con sus correspondientes archivos (proyectos nuevos)
a -> crear archivo
r -> renombrar
b -> para borrar archivo
c -> copiar
p -> pegar
:vsp -> te crea al lado una replica exacta del archivo (a la derecha)
:sp -> lo mismo que la anterior pero por abajo
espacio + c +h -> cheatSheet del NvChat
![[Pasted image 20240514233133.png]]

leader es espacio

Ahora vamos a tunear la rofi:
https://github.com/newmanls/rofi-themes-collection

git clone https://github.com/NvChad/starter ~/.config/nvim && nvim

```
git clone https://github.com/newmanls/rofi-themes-collection
cd rofi-themes-collection
```

![[Pasted image 20240515184531.png|1500]]

![[Pasted image 20240515185307.png]]

elegir + enter + alt +a

windows + d :
![[Pasted image 20240515185406.png]]

Descargaremos app para bloquear pantalla:
![[Pasted image 20240515185626.png]]

https://github.com/meskarune/i3lock-fancy

```
git clone https://github.com/meskarune/i3lock-fancy.git
```

![[Pasted image 20240515185831.png]]

![[Pasted image 20240515190148.png]]

![[Pasted image 20240515190235.png]]

![[Pasted image 20240515190453.png]]

![[Pasted image 20240515191024.png]]

windows + esc -> recargar los cambios

windows + shift +x -> activamos el locker

Para quitar el autocompletado de nvchad:
![[Pasted image 20240515191504.png]]

busqueda en nvchad por :
![[Pasted image 20240515191612.png]]

esc -> indicar inicio (este caso línea 76) + v + seleccionar hasta línea 123:
![[Pasted image 20240515191833.png]]

d (borrar)
u (deshacer)

Para que cada vez que abramos nvim se abra todo como nvchad, tenemos que configurlo:
![[Pasted image 20240515193126.png]]
![[Pasted image 20240515193148.png]]

![[Pasted image 20240515193228.png]]
![[Pasted image 20240515193338.png]]

![[Pasted image 20240515193413.png]]

Habría que repetir los pasos de abrir nvim, archivo .lua, y el autocompletado.

Tunearemos firefox:

1.- quitar historico y no permitir nunca -> en settings (buscar por historico)
2.- escribir en navegador about:config
![[Pasted image 20240515195304.png|1500]]
Así intentará a resolver sin buscar a google

3.- bajar addon wappalyzer -> https://addons.mozilla.org/en-US/firefox/addon/wappalyzer/
4.- bajar addon foxyproxy -> https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/
Crearemos un nuevo proxy:
![[Pasted image 20240515200209.png]]

![[Pasted image 20240515200702.png]]

![[Pasted image 20240515200842.png]]

![[Pasted image 20240515201024.png]]

La primera vez tendremos problemas con el certificado:
![[Pasted image 20240515201502.png]]

Así lo solventaremos:
![[Pasted image 20240515201557.png]]
guardamos el certificado.

Iremos a setting del navegador -> filtrar por certificates:
![[Pasted image 20240515201749.png]]
 -> view certificates -> Import
 ![[Pasted image 20240515201856.png]]

![[Pasted image 20240515201947.png]]

Instalar ranger ->  Para moverte por directorios.

```
sudo apt install ranger
```

![[Pasted image 20240515202717.png]]

![[Pasted image 20240515202738.png]]

