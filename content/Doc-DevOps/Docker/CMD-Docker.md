#### DOCKER DESKTOP Linux Mint 21.3, que está basado en Ubuntu 22.04 LTS ####
1. Primero, elimine cualquier repositorio de Docker existente:
`sudo rm /etc/apt/sources.list.d/docker.list*`

2. Actualice el índice de paquetes e instale los paquetes necesarios:
`sudo apt update`

>no sé si es relevante
Install required packages:
`sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release`

3. Añada la clave GPG oficial de Docker:
`sudo apt install ca-certificates curl gnupg`
`sudo install -m 0755 -d /etc/apt/keyrings`
`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg`
`sudo chmod a+r /etc/apt/keyrings/docker.gpg`

4. Configure el repositorio:
`echo \`
`"deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \`
`jammy stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`

`echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`
>creo que este funcionará si no lo hace el de arriba

5. Actualice el índice de paquetes:
`sudo apt update`

6. Ahora, intente instalar Docker:
`sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`

7. Una vez que Docker esté instalado, puede intentar instalar Docker Desktop:
`sudo apt install ./docker-desktop-amd64.deb`

SI PERMISO DENEGADO...
8. Primero, cambie los permisos del archivo para que sea legible por todos:
`sudo chmod 644 ~/Descargas/docker-desktop-amd64.deb`

9. Luego, cambie el propietario del archivo al usuario root:
`sudo chown root:root ~/Descargas/docker-desktop-amd64.deb`

10.Ahora, intente instalar Docker Desktop nuevamente:
`sudo apt install ~/Descargas/docker-desktop-amd64.deb`
____________________________________________
#### Comandos Docker
saber que iniciarlo en puerto 80
`docker run -d -p 80:80 docker/getting-started`

encontrar puerto específico
`sudo lsof -i -P -n | grep 80`

ver imagenes que tengo
`docker image ls`

ver todos los contenedores disponibles
`docker ps -a`

descarga una app para pruebas
`docker run -p 8080:80 -p 7080:7080 --name conBilling sotobotero/billingapp`

ver si está conectado al puerto 
`telnet localhost 8080`