#  Configuración de la maquina charlotte

  

###  Configuración de de polybar, bspwm , sxhkd y zsh

- Recuerda que la siguiente configuración esta pensada para dos monitores

**Monitor principal**

![pantalla-principal.jpg](https://raw.githubusercontent.com/skayblye/config-bspwm/main/img/01.png)
  

**Monitor secundario**

![monitor-secundario.jpg](https://raw.githubusercontent.com/skayblye/config-bspwm/main/img/02.png)

## Instalación de kitty

    sudo apt install kitty

Ahora nos moveremos la carpeta kitty a **.config**.

    mv kitty/ ~/.config

Listo, dentro de la carpeta kitty se encuentra dos archivos, uno contiene las configuraciones de kitty, como fuente, tamaño de letra, etc. El otro contiene solo los colores.
  
##  Instalación de la fuerte

> Me gusta instalar primero la zsh ya que sera donde nos estemos moviendo y sin auto completado y historial se vuelve pesado moverse en consola.  

Antes de descargar [powerlevel10k](https://github.com/romkatv/powerlevel10k) necesitamos instalar las fuentes para evitar problemas al momento de elegir un tema.

Nos dirijamos a [NerdFonts](https://www.nerdfonts.com/font-downloads) y descargamos la fuente **hack nerd font** y entraremos en la siguiente ruta.

	cd /usr/local/share/fonts/

Ahora nos moveremos la fuente a esa carpeta.

	sudo mv ~/Descargas/Hack.zip .

Lo descomprimimos y nos borramos el archivo .zip

	sudo unzip Hack.zip && rm Hack.zip

##  Instalación de powerlevel10k en zsh

>Nos dirigimos al repositorio de [powerlevel10k](https://github.com/romkatv/powerlevel10k) y verificamos si la instalación no cambio para evitar problemas.

  

Para instalar ejecutamos copiamos y ejecutamos lo siguiente

	git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
	echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc

Con eso echo solo escribimos **zsh** en consola y nos saltara el instalador, ahora es solo seguir los pasos que nos indican.

Una vez terminado el proceso vemos que ya tenemos zsh instalado pero si elegimos un tema que no es el **pure** veremos que tenemos unas cosas a la derecha de la consola, lo cual hay que quitar por que al tener mas de dos consolas abiertas se descuadra. Entrando al archivo p10k arreglaremos esto.

	nano .p10k.zsh

Dentro del archivo buscamos **Right prompt segments** y comentaremos todos los módulos, si nos interesa alguno de esos, escribiremos su nombre en los módulos de la izquierda **Left prompt segments**.

>Modulos recomendados: newline, command_execution_time, dir, context , status

terminado esto, tendremos que repetir el proceso de **instalación de powerlevel10k en zsh** con el usuario root o cualquier usuario que quiera la zsh.

Terminado la instalación en todos los usuarios procederemos a definir la zsh como shell por defecto a todos los usuarios

	usermod --shell /usr/bin/zsh root
	usermod --shell /usr/bin/zsh NombreDeUsuario

> si los cambios no se ven reflejados hay que reiniciar el sistema

ahora formaremos un enlace simbólico para no tener que editar dos **zshrc** (archivo de configuración de la zsh). Lo que haremos sera usar el archivo de configuración de nuestro usuario normal y compartirlo con root para que los dos compartan los plugins que instalemos a futuro.

	sudo ln -s -f /home/usuario/.zshrc .zshrc
	
> configuración de zshrc [aqui](https://github.com/SkayBlue/config-bspwm/blob/main/.zshrc)

  

por ultimo, puede que podemos tener un error al migrar a otro usuario con **su usuario**, para corregirlo ejecutamos lo siguiente como root.

	chown TUusuario:usuario /root
	chown TUusuario:usuario /root/.cache -R
	chown TUusuario:usuario /root/.local -R

  
##  Instalación de plugins en zsh

Instalaremos primero [bat](https://github.com/sharkdp/bat/releases), este toma la función del comando cat pero con colores, así que vamos a su pagina y descargamos el amd64.deb (actualmente la la versión mas reciente es la 0.21.0 y no da errores).

para instalarlo ejecutamos lo siguiente.

	sudo dpkg -i nombreDelArchivo.deb

y con esto tendríamos instalado bat y listo para funcionar, hay dos formas de usarlo, llamándolo como bat en consola y poniéndolo como alias en zrhrc para llamarlo cuando pongamos cat.

----

Lo siguiente que instalaremos sera [lsd](https://github.com/Peltoche/lsd/releases), al igual que el bat el sld remplaza al ls y nos lo muestra con colores y iconos, nos iremos a su pagina y nos descargaremos el amd64.deb (actualmente la versión mas reciente es la 0.22.0 y no da errores).

para instalarlo ejecutamos lo siguiente.

	sudo dpkg -i nombreDelArchivo.deb

al igual que con bat, con lsd podremos llamarlo así o poniendo un alias en la zshrc para ligarlo a nuestro ls.

---

Ahora instalaremos un buscador de rutas inteligente llamado [fzf](https://github.com/junegunn/fzf) al hacer **Ctrl + t** nos abrirá un buscador donde solo tendremos que escribir el nombre de lo que buscamos y nos dira la ruta donde se encuentra y con **Ctrl + r** nos abrirá un buscador con todos nuestros comandos ejecutados, para instalarlo ejecutamos lo siguiente.

  

	git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
	~/.fzf/install

>revisar la pagina para ver si el método de instalado no a cambiado y evitar problemas.

  

cunando lo ejecutemos nos preguntara algunas cosas, le pondremos en todo que si y la instalación finalizara, hay que recordar hacer la instalación con todos los usuarios por separado.

---

Por ultimo instalaremos un plugin para que nos agregue el comando sudo al principio de un comando al hacer **Esc** , esto es muy útil cuando tenemos un que ejecutar un comando largo y olvidamos ejecutar como root, nos vamos a su repositorio [sudo.plugin.zsh](https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/plugins/sudo/sudo.plugin.zsh)  

y antes de descargarlo hay que crear una carpeta donde deseemos tener nuestros prugins, en este caso va entrar el /usr/share, nos creamos la carpeta.

	sudo mkdir /usr/share/zsh-plugins && cd /usr/share/

ya adentro de la de la carpeta share, definiremos el dueño la carpeta zsh-plugins como nuestro usuario normal para no tener problemas con permisos.

	sudo chown usuario:usuario zsh-plugins && cd zsh-plugins

Ahora nos descargaremos el plugin dentro de la carpeta usando wget

  

	wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/plugins/sudo/sudo.plugin.zsh

  

Una vez descargado entraremos al archivo .zshrc y haremos un source con la ruta del donde esta el plugin, de manera que quedara asi: `source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh`

  

---

de manera opcional nos podemos descargar ranger , este cumple las funciones de caja pero se ejecuta desde consola.

	sudo apt install ranger

##  instalación de bspwn y sxhkd


>Los pasos de instalación pudieron cambiar, reucerda consultar la wilki de [bspwm](https://github.com/baskerville/bspwm/wiki) para prevenir errores.

Primero instalaremos los paquetes requeridos de bspwm

	sudo apt-get install libxcb-xinerama0-dev libxcb-icccm4-dev libxcb-randr0-dev libxcb-util0-dev libxcb-ewmh-dev libxcb-keysyms1-dev libxcb-shape0-dev

Ahora procedemos a clonar sus repositorios

  

	git clone https://github.com/baskerville/bspwm.git
	git clone https://github.com/baskerville/sxhkd.git

Una vez clonado hay que compilarlos.

	cd bspwm && make && sudo make install
	cd ../sxhkd && make && sudo make install
  
##  configuración de bspwm y sxhkd

Una vez todo instalado, hay que hacer la configuración, Ahora nos clonaremos el repositorio

	git clone https://github.com/SkayBlue/config-bspwm

Ejecutamos el siente comando para movernos y copiar la carpeta con los archivos de bspwm.

	cd .config/ && mv ~/Descargas/config-bspwm/bspwm .

Ejecutamos lo siguiente para darle permisos de ejecución.

	cd bspwn/ && chmod +x bspwmrc

algo que hay que configurar son los monitores y las ventanas, con el siguiente comando sabemos los monitores conectados y sus nombres

	xrandr -q | grep " connected" | cut -d ' ' -f1

sabiendo los nombres entramos al archivo bspwmrc.

	nano bspwmrc

y ahora donde dice **#monitores y configuración** editamos los nombres para coincidir con los nombres de nuestros monitores.

	bspc monitor EditaAqui_1 -n monitor0 -d 1 2 3 4

	bspc monitor EditaAqui_2 -n monitor1 -d 6 7 8 9

	bspc monitor EditaAqui_1 -s EditaAqui_2

lo siguiente es darle permisos al archivo que esta en a la carpeta scripts, este sirve para poder modificar el tamaño de las ventanas.

	cd scripts/ && chmod +x bspwm_resize
	
ya estando en la carpeta de usuario proseguimos a hacer lo mismo con la carpeta sxhkd

	cd .config/ && mv ~/Descargas/config-bspwm/sxhkd .

  

##  transparencias y fondo de pantalla

Ahora para gestionar transparencias instalaremos [picom](https://github.com/yshui/picom)

> visitar la pagina por si la instalación cambio y evitar errores.

actualizamos y instalamos las cosas que necesita antes de continuar con la instalación.

	sudo apt update && sudo apt install libxext-dev libxcb1-dev libxcb-damage0-dev libxcb-xfixes0-dev libxcb-shape0-dev libxcb-render-util0-dev libxcb-render0-dev libxcb-randr0-dev libxcb-composite0-dev libxcb-image0-dev libxcb-present-dev libxcb-xinerama0-dev libxcb-glx0-dev libpixman-1-dev libdbus-1-dev libconfig-dev libgl1-mesa-dev libpcre2-dev libpcre3-dev libevdev-dev uthash-dev libev-dev libx11-xcb-dev meson

teniendo todo listo, nos clonamos el repositorio.  

	git clone https://github.com/ibhagwan/picom.git

una vez clonado necesitamos compilarlo, para eso nos metemos dentro de la carpeta

	cd picom/
	git submodule update --init --recursive
	meson --buildtype=release . build
	ninja -C build

ahora ya teniendo todo listo para funcionar solo pasamos el archivo de configuración de picom.

	cd && cd .config/ && mv ~/Descargas/config-bspwm/picom .

y ya tendríamos transparencias. no necesitamos explicar mas ya que en el archivo bspwmrc ya se encuentra especificado su lanzamiento al iniciar.

---
Para tener un fondo de pantalla tendremos que instalar feh 

	sudo apt install feh


teniendo instalado feh nos dirigimos a nuestra carpeta bspwn para editar el archivo bspwmrc y definir el fondo de pantalla

	cd && nano ~/.config/bspwm/bspwmrc

ya estando dentro hay que editar la siguiente linea


	feh --bg-fill ~/Imágenes/fondos/1145320.png

Aquí lo que tendremos que cambiar sera la ruta donde se encuentra la imagen y el nombre de la imagen.

##  instalación de polybar

> visite la pagina de [polybar](https://github.com/polybar/polybar#installation) para verificar si la instalación no a cambiado.

  

primero instalamos todo lo que necesita para funcionar

  > Actualmente polybar esta disponible en muchas distros con repos oficiales, verifique primero si su distro esta en la lista.

	sudo apt install build-essential git cmake cmake-data pkg-config python3-sphinx python3-packaging libuv1-dev libcairo2-dev libxcb1-dev libxcb-util0-dev libxcb-randr0-dev libxcb-composite0-dev python3-xcbgen xcb-proto libxcb-image0-dev libxcb-ewmh-dev libxcb-icccm4-dev

ahora nos descargamos la polybar clonando el repositorio.
 
	 git clone --recursive https://github.com/polybar/polybar && cd polybar/

  

ya dentro de la carpeta toca compilarlo.

	mkdir build
	cd build
	cmake ..
	make -j$(nproc)
	
	# Optional. This will install the polybar executable in /usr/local/bin
	sudo make install
  

ya teniendo instalado polybar y funcionado podemos instalar temas.

---

vamos a usar [polybar themes](https://github.com/adi1090x/polybar-themes) ya que cuenta con muchos muy lindos.

>vista la pagina para verificar si la instalación no a cambiado.

primero nos clonamos el repositorio, nos metemos a la carpeta y le damos permisos al instalador.

	git clone --depth=1 https://github.com/adi1090x/polybar-themes.git && cd polybar-themes && chmod +x setup.sh

proseguimos a ejecutarlo,

	./setup.sh
	[*] Installing Polybar Themes...
	[*] Choose Style -

	[1] Simple
	[2] Bitmap


	[?] Select Option : 1


	[*] Installing fonts...
	[*] Creating a backup of your polybar configs...
	[*] Successfully Installed.

Al hacerlo veremos ese mensaje y señalamos la opción 1.

Con esto ya tendremos varios fondos pero el que usamos el el **forest**, para instarlo primero debemos borrar el que viene por defecto.

	cd && rm -rf ~/.config/polybar/forest

ya con el fondo borrado moveremos el que esta el en repositorio.

	cd ~/.config/polybar && mv ~/Descargas/config-bspwm/firest . && cd forest

estando dentro hay que darle permisos de ejecución.  

	chmod +x launch.sh

con eso solo queda darle permisos a los scripts del sistema.

	cd scripts/ && chmod +x *.sh

> con ese comando le hemos dado permisos a todos los .sh que estén en la carpeta. 

por ultimo para que funcione polybar-themes correctamente nos descargamos rofi y i3lock

	sudo apt install rofi i3lock


##  definir monitores

 
Para definir los monitores primero tenemos que saber cuales están conectados y sus nombres.

	xrandr -q | grep " connected" | cut -d ' ' -f1

Una vez teniendo el nombre de los monitores entraremos al archivo **config.ini** que se encuentra en la carpeta de de **forest**.

	cd && nano ~/.config/polybar/forest/config.ini

Estando dentro toca buscar el apartado **[bar/main]** y editaremos la parte donde dice **monitor**, toma en cuenta que **[bar/dos]** es el monitor secundario.

	monitor = ${env:MONITOR:Edita_aqui2}

Hay que repetir lo mismo pero para el monitor principal, buscamos **[bar/main]** y editaremos la parte donde dice **monitor**.  

	monitor = ${env:MONITOR:Edita_aqui1}


##  panel de login (opcional)

como panel de login estamos usando sddm, a la mitad de la instalación nos saldrá para elegir nuestro panel en caso que tengamos otro, seleccionamos sddm y seguimos con la instalación.

	sudo apt install sddm

con esto ya esta instalado pero no tienen ningún tema por lo cual se ve feo, asi que instalamos uno.

	sudo apt install sdd-theme-breeze

con esto ya tendríamos el sistema funcionando y listo.


