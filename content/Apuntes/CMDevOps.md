#!/bin/bash

### apt-get es mejor para desarrolladores que manejan scripts que apt. ####
### las diferentes versiones son soportadas por apt-get 
###### PARA BORRAR CUALQUIER SOFTWARE ######### 
	sudo -i
	sudo snap remove <tmcbeans> (ejemplo)
###### SABER MI VERSION DE LINUX #######
	cat /etc/lsb-release

###### SNAPD INSTALL, PARA QUE SE DESACTIVE LA NO-ACTUALIZACION AUTOMÁTICA
	sudo rm /etc/apt/preferences.d/nosnap.pref
	sudo apt update && sudo apt install snapd

###### update package list #########
	sudo apt-get update

###### install pip #########
	sudo apt-get install python-pip

###### install python ######
	sudo apt install pythonpy

###### install Visual Code ######
	apt-get install code
	sudo snap install code --classic

###### install IntelliJ IDEA ######
	apt-get install intellij-idea-community // NO FUNCIONA, LEE LISTA DE PAQUETES Y CREA ARBOL DE DEPENDENCIAS PERO NO LOCALIZA EL PACK
	sudo snap install intellij-idea-community --classic

###### install Flameshot ######
	sudo apt-get install flameshot

###### install Git ######
	sudo apt-get install git
	sudo apt-get install git-all

###### install GitKraken desktop ######
	sudo snap install gitkraken --classic

###### install Brave ###### 
	sudo apt install curl

	sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg

	echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg] https://brave-browser-apt-release.s3.brave.com/ stable main"|sudo tee /etc/apt/sources.list.d/brave-browser-release.list

	sudo apt update

###### install OpenJDK 8 ######
	sudo apt-get install openjdk-8-jdk

###### install OpenJDK 19 ######
	sudo apt-get install openjdk-19-jdk
###### CAMBIAR DE VERSIONES
	sudo update-alternatives --config java

###### install netBeans ######
	sudo add-apt-repository "deb http://archive.ubuntu.com/ubuntu focal universe"
	sudo apt update
	sudo apt install netbeans

###### install TMCBEANS ######
	sudo apt-get update
	sudo apt-get install openjdk-11-jdk
	sudo update-alternatives --config java
	sudo snap install --classic tmcbeans
	snap run tmcbeans

###### install tmcBeans ######
	sudo snap install tmcbeans --classic (NO CORRECT)
	sudo snap install --classic tmcbeans

###### install Eclipse ######
	sudo apt-get install eclipse
	sudo snap install eclipse --classic

###### install Telegram ###### 
	sudo apt-get install telegram-desktop
	sudo snap install telegram-desktop

###### install Sublime Text ######
	sudo apt-get install sublime-text
	sudo snap install sublime-text --classic

###### install KeePassXC ######
	sudo apt-get install keepassxc

###### install Kubernetes ######
	sudo snap install kubectl --classic

###### install Kubernetes agent run on each node ######
	sudo snap install kubelet --classic

###### install Prometheus ######
	sudo snap install prometheus

###### install Chrome ######
	wget -c https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
	sudo apt-get update
	sudo apt-get install libappindicator1
	sudo dpkg -i google-chrome-stable_current_amd64.deb
					
	sudo apt-get install google-chrome-stable (no localiza)

###### install Node.js ###### 
	sudo apt-get install nodejs

###### install npm ###### 
	sudo apt-get install npm

##### install Maven ###### 
	sudo apt-get install maven

###### install MarkText ######
	sudo snap install marktext

###### install Postman ###### 
	sudo apt-get install postman
	sudo snap install postman

###### install GitHub Desktop ###### 
	wget -qO - https://apt.packages.shiftkey.dev/gpg.key | gpg --dearmor | sudo tee /usr/share/keyrings/shiftkey-packages.gpg > /dev/null
	sudo sh -c 'echo "deb [arch=amd64 signed-by=/usr/share/keyrings/shiftkey-packages.gpg] https://apt.packages.shiftkey.dev/ubuntu/ any main" > /etc/apt/sources.list.d/shiftkey-packages.list'
	sudo apt update && sudo apt install github-desktop

###### install Onlyoffice ######
	sudo snap install onlyoffice-desktopeditors

###### install Discord ###### 
	sudo snap install discord

###### install notion ######
	sudo snap install notion-snap-reborn

###### install nextcloud ######
	sudo snap install nextcloud

###### install docker ######
	sudo snap install docker

###### install Obsidian ###### 
	sudo snap install obsidian --classic

###### install slack ######
	sudo snap install slack

###### install MongoDB ###### 
	https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/

	### cat /etc/lsb-release // ver la distribución de linux
	### sudo apt-get install gnupg curl
	### echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
	### sudo apt-get update
	### sudo apt-get install -y mongodb-org
	### sudo systemctl start mongod


	### para instalar la shell >>> https://www.mongodb.com/docs/mongodb-shell/install/
	- lsb_release -dc // ver la distribución de linux
	- cat /etc/lsb-release // ver la distribución de linux
	- sudo apt-get remove <path del paquete> //desinstalar paquete
	- curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor


	- wget -qO- https://www.mongodb.org/static/pgp/server-7.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-7.0.asc
	- sudo apt-get install gnupg
	- wget -qO- https://www.mongodb.org/static/pgp/server-7.0.asc | sudo tee /etc/apt/trusted.gpg.d/server-7.0.asc
	-# echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
	-# sudo apt-get update
	-# sudo apt-get install -y mongodb-mongosh
	-- sudo apt-get install gnupg curl
	-- curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
	-- echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
	-- sudo apt-get update
	-- sudo apt-get install -y mongodb-org

	-- sudo nano /etc/systemd/system/mongodb.service
[Unit]
Description=High-performance, schema-free document-oriented database
After=network.target

[Service]
User=mongodb
ExecStart=/usr/bin/mongod --quiet --config /etc/mongod.conf

[Install]
WantedBy=multi-user.target

sudo systemctl daemon-reload // Recarga la configuración del servicio 
sudo systemctl restart mongod.service // Reinicia el servicio
sudo systemctl daemon-reload
sudo systemctl enable mongod



--sudo systemctl start mongodb
--sudo systemctl status mongodb
-- service mongod start
-- mongo


	- mongosh --version
	- mongosh // se conecta a la db local
	- show dbs // muestra la db que hay

	#### instalar mongodb compass >>> https://www.mongodb.com/docs/compass/current/install/
	- wget https://downloads.mongodb.com/compass/mongodb-compass_1.43.4_amd64.deb
	- sudo apt install ./mongodb-compass_1.43.4_amd64.deb
	If your Linux distribution does not support using apt for installing local .deb files, run the following lines to install MongoDB Compass:
	- sudo dpkg -i mongodb-compass_1.43.4_amd64.deb

	sudo apt-get install -f # This installs required compass dependencies
	- mongodb-compass
	

	### CHECK INSTALLATION MONGODB ######
	- mongod --version
	- dpkg -l | grep mongodb // dónde está instalada

	### CHECK IF ITS ON MY PC ######
	- sudo systemctl status mongod
	(If MongoDB is running, you should see a message that says "active (running)". 
	If MongoDB is not running, you should see a message that says "inactive (dead)".)

	######FOR STARTING RUNNING ######
	- sudo systemctl start mongod

	- apt-key list
	- echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
	- sudo apt update
	- sudo apt install mongodb-org

	◘◘◘◘◘  IF NOT CONNECTING // ERROR connect ECONNREFUSED 127.0.0.1:27017 
	- sudo service mongod start // NO SALE

	#### INSTALAR DATABASE TOOLS
	https://www.mongodb.com/docs/database-tools/installation/installation-linux/
	### DESCARGAR
	-sudo apt install ./mongodb-database-tools-*-100.9.5.deb
	Be sure to include the leading ./ in the command above, which instructs apt to look for this file in the local directory instead of searching remote repositories.

	### CHECK IF ITS ON MY PC ######
	- sudo dpkg -l mongodb-database-tools

###### install H2 database (no se instala) ######
	- sudo apt-get install h2database

###### install Spotify ######
	sudo snap install spotify

###### install Tor ###### 
	sudo apt-get install tor

###### install VirtualBox (no verify) ######
	sudo apt-get install virtualbox

###### install VMware (no verify) ######
	sudo apt-get install vmware

###### install VLC ######
	sudo snap install vlc

###### install Foxit Reader (no se instala) ######
	sudo apt-get install foxitreader

###### install Quarto (no se instala) ######
	sudo apt-get install quarto

###### install libreoffice (instalado por defecto) ######
	sudo snap install libreoffice

###### ver lista de los programas instalados por snapd ######
	snap list
	snap version (verificar instalación)
	sudo snap remove tmcbeans (delete)
	sudo rm -rf /var/snap/tmcbeans (delete archivos)
	sudo chown -R crhacker7:crhacker7 /snap/tmcbeans (dar permisos)

###### ACTUALIZAR SIN BACKUP REPETIDO ######

	sudo snap refresh --classic --no-backup //me actulizará sin que me guarde los archivos antiguos
	sudo snap refresh <gnome-calculator> --classic --no-backup 
	sudo apt update //no usarlo mucho porque me saldran multiples archivos 

###### TABLE PLUS Ubuntu 20.04 X86_64 (updated in web)########
	# Add TablePlus gpg key
	wget -qO - https://deb.tableplus.com/apt.tableplus.com.gpg.key | sudo apt-key add -

	# Add TablePlus repo
	sudo add-apt-repository "deb [arch=amd64] https://deb.tableplus.com/debian/20 tableplus main"

	# Install
	sudo apt update
	sudo apt install tableplus

###### PARA EJECUTAR Y DESZIPEAR ARCHIVOS ######
	◘◘◘◘◘ tar -xzvf nombre_del_archivo.tar.gz
	./nombre_del_archivo // si termina en [.sh]
	chmod +x nombre_del_archivo //si no tiene permisos de ejecución:
	
	◘◘◘◘◘ EJECUTAR ARCHIVOS .DEB
	sudo dpkg -i nombre_del_archivo.deb
	sudo apt-get install -f (SI PROBLEMAS DE DEPENDENCIAS) RESUELVE AUTOMATICAMENTE LAS DEPENDENCIAS
	sudo dpkg -i nombre_del_archivo.deb
    sudo apt update
    sudo apt install mysql-server (o cualquier app)
    dpkg -l | grep mysql (ver si se ha instalado)
    sudo apt -f install (despues del fallo de dep)

| Herramienta      | Instala `.deb` | Resuelve dependencias       | Recomendado para...            |
| ---------------- | -------------- | --------------------------- | ------------------------------ |
| `dpkg -i`        | ✅              | ❌                           | Casos muy controlados          |
| `apt -f install` | ❌              | ✅ (solo después de errores) | Para arreglar problemas        |
| `gdebi`          | ✅              | ✅                           | ✅ Instalación segura de `.deb` |
	
###### INSTALA DESDE EL .DEB Y ARREGLA LAS DEPENDENCIAS.
sudo apt install gdebi-core
sudo gdebi mysql-workbench-community_8.0.43-1ubuntu22.04_amd64.deb


	sudo apt install ./docker-desktop-amd64.deb

####### DOCKER DESKTOP Linux Mint 21.3, que está basado en Ubuntu 22.04 LTS ############
1. Primero, elimine cualquier repositorio de Docker existente:
sudo rm /etc/apt/sources.list.d/docker.list*

2. Actualice el índice de paquetes e instale los paquetes necesarios:
sudo apt update

3. Añada la clave GPG oficial de Docker:
sudo apt install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

4. Configure el repositorio:
echo \
"deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
jammy stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

5. Actualice el índice de paquetes:
sudo apt update

6. Ahora, intente instalar Docker:
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

7. Una vez que Docker esté instalado, puede intentar instalar Docker Desktop:
sudo apt install ./docker-desktop-amd64.deb

SI PERMISO DENEGADO...
8. Primero, cambie los permisos del archivo para que sea legible por todos:
sudo chmod 644 ~/Descargas/docker-desktop-amd64.deb

9. Luego, cambie el propietario del archivo al usuario root:
sudo chown root:root ~/Descargas/docker-desktop-amd64.deb

10.Ahora, intente instalar Docker Desktop nuevamente:
sudo apt install ~/Descargas/docker-desktop-amd64.deb

sqlite

Make the file executable, open the command line and run: chmod u+x devtools.sh. 
You are giving permissions to execute this file: it will grant only the owner of that file execution permissions.

