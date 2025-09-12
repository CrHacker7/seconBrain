############ Docker installation################
DOCKER ENGINE

-version de docker
	sudo docker -v

version docker compose
	sudo docker compose version

-verificar si esta instalado
	sudo docker run hello-world

_______________Post-installation-steps________________
	sudo groupadd docker
	sudo usermod -aG docker $USER

______________log-rotation____________________________
IMPORTANTE SINO SE CAEN LOS DOCKERS POR FALTA DE ESPACIO

	sudo nano /etc/docker/daemon.json

{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}

REINICIAR

permite ver los contenedores en todos sus estados
	docker ps -a
