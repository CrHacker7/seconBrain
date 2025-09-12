
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
_______________________________________________________________________
CAMBIAR DE VERSIONES DE NODE

nvm install 14.21.3: 
Instala la versión 14.21.3 de Node.js en tu sistema.
nvm use 14.21.3: Cambia a utilizar la versión 14.21.3 de Node.js.
nvm alias default 14.21.3: 
Establece la versión 14.21.3 como la predeterminada, de modo que se use cada vez que inicies una nueva sesión.

Ahora, cuando ejecutes node --version, deberías ver la versión 14.21.3.