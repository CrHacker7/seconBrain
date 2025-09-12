1. delete the
#### Uso de Builds Multietapa (Multi-Stage Builds)
- Por qué es importante: Separar el entorno de construcción del entorno de producción asegura que solo se incluyan los artefactos necesarios en la imagen final, reduciendo el tamaño y aumentando la seguridad al eliminar herramientas de desarrollo.
Ejemplo: Construir en una imagen base con herramientas de desarrollo, luego copiar solo lo esencial a una imagen más pequeña y limpia.

#### Elegir Imágenes Base Slim Verificadas
- Por qué es importante: Usar imágenes base más ligeras (como Alpine o variantes Slim) reduce la superficie de ataque y el tamaño de la imagen. Las imágenes oficiales y verificadas ofrecen seguridad adicional.
Ejemplo: En lugar de usar Ubuntu, usar python:3.9-slim.
#### Aprovechar el Caché de Capas
- Por qué es importante: El orden de las instrucciones en el Dockerfile impacta en el uso del caché, lo que puede acelerar las construcciones y reducir la cantidad de trabajo innecesario. Al poner las dependencias primero, Docker puede reutilizar capas cuando solo cambian partes del código.
Ejemplo: Colocar COPY requirements.txt antes de copiar todo el código para evitar reconstrucciones innecesarias.
#### Reducir el Número de Capas
- Por qué es importante: Cada comando en el Dockerfile crea una capa. Reducir el número de capas puede resultar en imágenes más pequeñas y en una construcción más rápida.
Ejemplo: Combinar comandos como apt-get update y install en un solo paso para evitar capas innecesarias.
#### Nunca Ejecutar Imágenes como Usuario Root
- Por qué es importante: Ejecutar contenedores como root expone a la aplicación a vulnerabilidades. Usar un usuario no privilegiado mejora la seguridad.
Ejemplo: Crear un usuario no root (USER myuser) y usarlo para ejecutar la aplicación dentro del contenedor.
#### Escanear Imágenes en Busca de Vulnerabilidades
- Por qué es importante: Escanear imágenes regularmente ayuda a identificar vulnerabilidades conocidas, lo que permite corregirlas antes de que se implementen en producción.
Ejemplo: Utilizar herramientas como Trivy para escanear y obtener un informe sobre las vulnerabilidades de la imagen.
#### Mantener las Dependencias Actualizadas: 
- Actualizar regularmente las imágenes base y las dependencias para incluir parches de seguridad.
#### Limitar el Número de Paquetes Instalados:
- Instalar solo los paquetes necesarios para ejecutar la aplicación, evitando paquetes superfluos que aumenten el tamaño y la superficie de ataque.
#### Tamaño y rendimiento: 
- Reducir el tamaño de las imágenes mejora los tiempos de despliegue y optimiza los recursos.
#### Seguridad: 
- Minimizar la superficie de ataque y no usar privilegios elevados reduce los riesgos de explotación.
#### Mantenibilidad: 
- Escanear imágenes y mantenerlas actualizadas asegura que las vulnerabilidades conocidas sean mitigadas rápidamente.

1. candado
2. layers
3. combination of cmd for using caché used
4. no user root user
5. scan image
6. update dependencies

#### Para agregar tu usuario al grupo docker
**Problemas con el daemon:** docker: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Head "http://%2Fvar%2Frun%2Fdocker.sock/_ping": dial unix /var/run/docker.sock: connect: permission denied.
Agregar usuario
- sudo usermod -aG docker $USER
Recargar para que surta efecto sin cerrar sesión
- newgrp docker