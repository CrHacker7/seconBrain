instalar Node.js, npm y nvm

Actualizar el sistema:
sudo apt update
sudo apt upgrade

Instalar las dependencias necesarias:
sudo apt install software-properties-common
sudo apt install build-essential libssl-dev

Instalar nvm (Node Version Manager):
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

Después de esto, cierra y vuelve a abrir la terminal.

Verificar la instalación de nvm:
nvm --version

Instalar la última versión de Node.js a través de nvm:
nvm install node

Verificar la instalación de Node.js y npm:
node --version
npm --version

(Opcional) Establecer una versión predeterminada de Node.js:
nvm use node
nvm alias default node
________________________________________________________________________________________________________________________
CAMBIAR DE VERSIONES DE NODE

nvm install 14.21.3: 
Instala la versión 14.21.3 de Node.js en tu sistema.
nvm use 14.21.3: Cambia a utilizar la versión 14.21.3 de Node.js.
nvm alias default 14.21.3: 
Establece la versión 14.21.3 como la predeterminada, de modo que se use cada vez que inicies una nueva sesión.

Ahora, cuando ejecutes node --version, deberías ver la versión 14.21.3.



Actualizar el sistema:
sudo apt update
sudo apt upgrade

Instalar las dependencias necesarias:
sudo apt install software-properties-common
sudo apt install build-essential libssl-dev

Instalar nvm (Node Version Manager):
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

Después de esto, cierra y vuelve a abrir la terminal.

Verificar la instalación de nvm:
nvm --version

Instalar la última versión de Node.js a través de nvm:
nvm install node

Verificar la instalación de Node.js y npm:
node --version
npm --version

(Opcional) Establecer una versión predeterminada de Node.js:
nvm use node
nvm alias default node
________________________________________________________________________________________________________________________
CAMBIAR DE VERSIONES DE NODE

nvm install 14.21.3: 
Instala la versión 14.21.3 de Node.js en tu sistema.
nvm use 14.21.3: Cambia a utilizar la versión 14.21.3 de Node.js.
nvm alias default 14.21.3: 
Establece la versión 14.21.3 como la predeterminada, de modo que se use cada vez que inicies una nueva sesión.

Ahora, cuando ejecutes node --version, deberías ver la versión 14.21.3.

_______________________________________________________________________________________________________________
Instalación de NVM

Use curlo wgetpara descargar el script
	curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

O bien utilizas wget:
	wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

INSTALAR MEDIANTE REPOSITORIO-SCRIPT
https://github-com.translate.goog/nvm-sh/nvm/blob/v0.39.7/install.sh?_x_tr_sl=en&_x_tr_tl=es&_x_tr_hl=es&_x_tr_pto=rq

Ejecutar el script de instalación de NVM : el comando wgeto curldescarga el script y lo ejecuta de una sola vez. 
El script clona el repositorio de NVM en ~/.nvmy agrega las líneas de origen a su perfil 
( ~/.bash_profile, ~/.zshrc, ~/.profileo ~/.bashrc).
Verificar la instalación : Cierre su terminal y abra uno nuevo. Verifique que NVM esté instalado y funcionando con el siguiente comando:
	command -v nvm

Comando no encontrado : 
si la terminal dice command not founddespués de ejecutarlo command -v nvm, 
significa que no puede encontrar el nvmcomando. Es posible que deba cerrar y volver a abrir la terminal o reiniciar su computadora. 

Sin Curl o Wget : si su máquina no tiene curlo wget, deberá instalar uno de ellos para descargar el script de instalación de NVM.

Instalación de la última versión
	nvm install node

Instalación de una versión específica de Node.js
	nvm install 14.15.1

El nvm install comando descarga la versión especificada de Node.js y npm, lo que le permite usarlos inmediatamente.

Listado de todas las versiones de Node.js disponibles (NO ESTAN EN MI PC)
	nvm ls

Para ver todas las versiones disponibles para la instalación, utilice el nvm ls-remotecomando:
	nvm ls-remote (MILES DE VERSIONES)

cambiar las versiones de Node.js
	nvm use 21.7.1

ambiar su versión PREDETERMINADA utilizando el siguiente comando 
	nvm alias default 14.5.1

Verificar la versión actual
	nvm current
