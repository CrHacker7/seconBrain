# Commandos básico y utiles para el curso

## Índice

- [Commandos básico y utiles de redes y contenedores](#commandos-básico-y-utiles-para-el-curso)
  - [Índice](#índice)
  - [Network tools](#network-tools)
    - [Linux](#linux)
    - [Windows Power shell](#windows-power-shell)
  - [**Docker**](#docker)
    - [Trabajando con imágenes del container registry](#trabajando-con-imágenes-del-container-registry)
    - [Construyendo Imágenes](#construyendo-imágenes)
    - [Comandos Avanzados para Imágenes](#comandos-avanzados-para-imágenes)
    - [Comandos para la Gestión Básica de Contenedores](#comandos-para-la-gestión-básica-de-contenedores)
    - [Trabajando con Redes y Volúmenes](#trabajando-con-redes-y-volúmenes)
    - [Interactuando con Contenedores](#interactuando-con-contenedores)
    - [Docker Compose](#docker-compose)
    - [Administración de Redes y Volúmenes](#administración-de-redes-y-volúmenes)
    - [Comandos para Información y Estadísticas](#comandos-para-información-y-estadísticas)
    - [Comandos para Seguridad y Control de Acceso](#comandos-para-seguridad-y-control-de-acceso)
    - [Comandos para Configuraciones Avanzadas](#comandos-para-configuraciones-avanzadas)

## Network tools

### Linux 
* Install this tool to management network options
```shell
sudo apt install net-tools 
sudo apt-get install telnet
```
* Check ports listening
```shell
netstat -n --all --tcp
```
* Find specific port 
```shell
sudo lsof -i -P -n | grep 9000
```
* Testing socket whit telnet, teh format telnet ipaddress port
```shell
telnet localhost 5433
```

 ### Windows Power shell 

* Check ports listening
```powershell
Get-NetTCPConnection
```
* Obtener todas las conexiones TCP
```powershell
$connections = Get-NetTCPConnection
```
* Filtrar conexiones en estado 'Listen' o 'Established'*
```powershell
$filteredConnections = $connections | Where-Object {($_.State -eq 'Listen') -or ($_.State -eq 'Established')}
```
* Obtener información detallada de cada conexión*
```powershell
$connectionDetails = $filteredConnections | ForEach-Object {
    $localPort = $_.LocalPort
    $remotePort = $_.RemotePort
    $state = $_.State
    $processId = $_.OwningProcess
    $process = Get-Process -Id $processId
    $processName = $process.ProcessName

    [PSCustomObject]@{
        LocalPort = $localPort
        RemotePort = $remotePort
        State = $state
        Application = $processName
    }
}
```
* Mostrar la información en la consola
```powershell
$connectionDetails
```
* Find specific port 
```powershell
Get-NetTCPConnection | Where-Object {$_.LocalPort -eq 9000}
```
* Testing socket whit telnet, teh format telnet ipaddress port
```powershell
Test-NetConnection -ComputerName localhost -Port 5433
```

## **Docker**
### Trabajando con imágenes del container registry

- **Descargar una imagen desde Docker Hub:**
```shell 
#docker pull nombre_de_la_imagen
docker run -d -p 80:80 docker/getting-started
```
- **Listar imágenes locales:**
```shell 
docker image ls
```
- **Crear y ejecutar un contenedor a partir de una imagen:**
```shell 
#docker run [opciones] nombre_del_contenedor nombre_de_la_imagen
docker run -p 8080:80 -p 7080:7080 --name containerBilling sotobotero/billingapp
```
- **crear un contenedor de postgres y una base de datos en el mismo comando:**
```shell
docker run --ulimit memlock=-1:-1 -d --name postgres -e POSTGRES_USER=sa -e POSTGRES_PASSWORD=admin -e POSTGRES_DB=product_db -p 5432:5432 postgres:13.3
```
- **crear un contenedor de Mysql y una base de datos en el mismo comando:**
```shell
docker run -d  --name mysql -e MYSQL_ROOT_PASSWORD=admin  -e MYSQL_USER=sa -e MYSQL_PASSWORD=qwerty  -e MYSQL_DATABASE=product_db -p 3306:3306   mysql:latest
# in order to conect you need to set this property form client as true allowPublicKeyRetrieval=true
```
- **Listar contenedores en ejecución:**
```shell 
docker ps
```
- **Listar todos los contenedores (incluso los detenidos):**
```shell 
docker ps -a
```
- **Detener todos los contenedores:**
```shell 
docker stop $(docker ps -q)
```
- **Iniciar todos los contenedores:**
```shell 
docker start $(docker ps -a -q)
```
- **Eliminar todos los contenedores:**
```shell 
docker rm $(docker ps -a -q)
```
### Construyendo Imágenes

- **Construir una imagen desde un Dockerfile:**
 ```shell
 #docker build -t nombre_de_la_imagen [opciones y parametros] .
 docker build -t billingapp --no-cache --build-arg JAR_FILE=target/*.jar .
```
- **Construir una imagen con un contexto específico:**
 ```shell
  docker build -t nombre_de_la_imagen -f ruta/Dockerfile .
```
### Comandos Avanzados para Imágenes

- **Etiquetar una imagen:**
 ```shell
 docker tag nombre_de_la_imagen nueva_etiqueta
```
- **Loguears en un registry:**
 ```shell
docker login # te pedirá usuario y contraseña
 ```
- **Subir una imagen a Docker Hub:**
 ```shell
  docker push nombre_de_la_imagen
```
- **Descargar una imagen con una etiqueta específica:**
 ```shell
  docker pull nombre_de_la_imagen:etiqueta
```
- **Desloguearse de un registry:**
 ```shell
docker logout
 ```
### Comandos para la Gestión Básica de Contenedores

- **Detener un contenedor en ejecución:**
```shell 
docker stop nombre_del_contenedor
```

- **Iniciar un contenedor detenido:**
```shell 
docker start nombre_del_contenedor
```
- **Eliminar un contenedor:**
```shell 
docker rm nombre_del_contenedor
```
- **Eliminar una imagen local:**
```shell
docker image rm nombre_de_la_imagen
```
### Trabajando con Redes y Volúmenes

- **Crear una red personalizada:**
 ```shell 
 docker network create nombre_de_la_red
```
- **Listar redes:**
 ```shell
 docker network ls
```
- **Crear un volumen:**
 ```shell
 docker volume create nombre_del_volumen
```
- **Listar volúmenes:**
 ```shell
 docker volume ls
```
### Interactuando con Contenedores

- **Ejecutar un comando en un contenedor en ejecución:**
 ```shell
 #docker exec -it nombre_del_contenedor comando
docker exec -it localbillingApp sh
```
- **Copiar archivos entre el host y un contenedor:**
 ```shell
  docker cp archivo.txt nombre_del_contenedor:/ruta/destino
  ```
### Docker Compose

- **Iniciar contenedores definidos en un archivo `docker-compose.yml`:**
 ```shell
  docker-compose up
```
- **Escalar servicios en Docker Compose:**
 ```shell
  docker-compose scale servicio=num_instancias
```
- **Detener y eliminar contenedores definidos en Docker Compose:**
 ```shell
  docker-compose down
```
- **Ver logs de servicios en Docker Compose:**
 ```shell
  docker-compose logs servicio
```
### Administración de Redes y Volúmenes

- **Eliminar una red:**
 ```shell
  docker network rm nombre_de_la_red
```
- **Eliminar un volumen:**
 ```shell
  docker volume rm nombre_del_volumen
```
- **Inspeccionar una red:**
 ```shell
 docker network inspect nombre_de_la_red
```
### Comandos para Información y Estadísticas

- **Ver detalles de un contenedor:**
 ```shell
  docker inspect nombre_del_contenedor
```
- **Obtener estadísticas de uso de recursos de un contenedor:**
 ```shell
  docker stats nombre_del_contenedor
```
- **Obtener estadísticas de uso de recursos de todos los contenedores:**
 ```shell
  docker stats
```
### Comandos para Seguridad y Control de Acceso

- **Crear un perfil de control de acceso (se requiere Docker EE):**
 ```shell
  docker trust key generate nombre_de_perfil
```
- **Especificar qué usuarios o equipos pueden o no usar Docker:**
 ```shell
  docker trust key load --alias nombre_de_perfil.pem --pem --root /etc/docker/pki/tls/private
```

- **Inhabilitar Docker para usuarios no autorizados (se requiere Docker EE):**
 ```shell
  docker trust signer remove nombre_de_perfil
```
### Comandos para Configuraciones Avanzadas

- **Crear un contenedor con un archivo de configuración personalizada:**
 ```shell
  docker run -v ruta/local:/ruta/contenedor -e variable_de_entorno=valor nombre_de_la_imagen
```
- **Especificar recursos de sistema para un contenedor (CPU, memoria, etc.):**
 ```shell
 docker run --cpu-shares=512 -m 256m nombre_de_la_imagen
```
- **Elimina todos los contenedores detenidos e imagenes que no esten en uso**
 ```shell
docker system prune --all
 ```
 - **Elimina todos los volumenes que no esten en uso**
 ```shell
docker volume prune
 ```
 - **Elimina todas las redes que no esten en uso, menos las por defecto (bridge, none,host)**
 ```shell
docker network prune
 ```

## **Rangos de Puertos y Buenas Prácticas para su Gestión**

En redes y aplicaciones, los rangos de puertos se dividen en tres categorías principales según su uso y asignación. Aquí un resumen de los rangos típicos:

* **Puertos bien conocidos (0-1023):**
  - Reservados para servicios estándar y aplicaciones bien conocidas.
  - Solo los procesos del sistema o con privilegios elevados pueden usarlos.
  - Ejemplos:
    - HTTP: 80
    - HTTPS: 443
    - FTP: 21

* **Puertos registrados (1024-49151):**
  - Asignados para aplicaciones registradas pero no reservados exclusivamente.
  - Utilizados frecuentemente para aplicaciones de usuario específicas.
  - Ejemplo:
    - Docker Engine API: 2375 (sin TLS), 2376 (con TLS).

* **Puertos dinámicos o privados (49152-65535):**
  - Definición técnica: Estos puertos se asignan dinámicamente por el sistema operativo para conexiones temporales o aplicaciones sin requisitos específicos de puerto.
  - Uso en la industria: Son ideales para aplicaciones que no requieren exposición externa ni puertos fijos, como servicios internos en redes privadas o nodos intermedios.

  Aunque este tipo de puertos dinámicos son técnicamente válidos para usar en aplicaciones de negocio como microservicios y contenedores por ejemplo, no son recomendados porque dificultan:

  - La depuración: Si el puerto cambia constantemente, rastrear errores es más complejo.
  - La configuración de balanceadores de carga: Herramientas como NGINX o Traefik necesitan puertos predecibles.
  - La documentación: Estándares claros ayudan a nuevos miembros del equipo.

* **Puertos personalizados para entornos específicos:**
Aunque los puertos dinámicos son técnicamente adecuados, la industria TI utiliza rangos específicos por contexto para facilitar la gestión y depuración. Estos rangos pueden variar según la organización, y pese a que no es un  estándar, es una buena práctica seguida por muchas empresas.

    - Rangos sugeridos:
      - **Desarrollo (dev):** 3000-3999
      - **Pruebas (staging):** 4000-4999
      - **Producción (prod):** 8000 en adelante.

- Razonamiento práctico:
  - Facilitan la identificación de servicios por entorno.
  - Evitan conflictos en configuraciones compartidas.
  - Mejoran la documentación y estandarización de despliegues.

Puerto interno estándar en la aplicación:
Con el auge de los contenedores, las aplicaciones modernas suelen definir un puerto interno fijo en su configuración (por ejemplo, 8080). Este puerto no varía entre entornos porque representa el servicio dentro del contenedor.

Luego, el puerto interno se mapea a un puerto externo en el host para permitir el acceso al servicio desde fuera del contenedor. Por ejemplo, el puerto 8080 interno de un contenedor puede mapearse al puerto 80 del host.

Aunque los gestores de contenedores como Docker o Kubernetes admiten tanto puertos dinámicos como puertos fijos, se recomienda usar puertos fijos para facilitar la integración con herramientas y servicios externos, como sistemas de registry and discovery (Zookeeper, Eureka), balanceadores de carga, firewalls, y herramientas de monitoreo.

En Kubernetes: asignaciones predecibles de puertos
En Kubernetes, esta práctica se denomina asignaciones predecibles de puertos. Se recomienda especialmente en entornos de producción para mejorar:

La gestión de servicios: Simplifica la configuración de otros recursos como Services o Ingress.
La depuración de errores: Hace que las conexiones y rutas sean más fáciles de rastrear y resolver.


  ## Buenas Prácticas para la Gestión de Puertos

  ### Definición de Rangos de Puertos por Entorno

  Es fundamental definir rangos de puertos claros y específicos para cada entorno (desarrollo, pruebas, producción) para facilitar la gestión y evitar conflictos.

  ### Uso de Variables de Entorno

  Utiliza variables de entorno para definir los puertos de manera flexible y adaptable a diferentes entornos. Esto permite cambiar configuraciones sin modificar el código fuente.

  ### Limitación de Exposición de Puertos en Producción

  En producción, limita la exposición de puertos solo a los necesarios. Utiliza proxies inversos y firewalls para controlar el acceso y mejorar la seguridad.

  ### Centralización de Accesos con Proxies Inversos

  Implementa proxies inversos como NGINX o Traefik para manejar el enrutamiento y la administración de múltiples servicios. Esto permite exponer un único punto de entrada y facilita la gestión de certificados SSL y la configuración de balanceo de carga.

  #### Ejemplo de Subdominios:

  - **Dev:** dev.api.example.com
  - **Staging:** staging.api.example.com
  - **Prod:** api.example.com

  ### Configuración de Firewall

  Configura reglas de firewall que limiten el acceso a puertos específicos, especialmente en entornos sensibles. Asegúrate de que solo los servicios y usuarios autorizados puedan acceder a los puertos críticos.

  ### Resumen de Buenas Prácticas

  1. **Define rangos de puertos claros por entorno.**
  2. **Usa variables de entorno para flexibilidad.**
  3. **Limita la exposición de puertos en producción.**
  4. **Centraliza accesos con proxies inversos.**
  5. **Mantén reglas de firewall estrictas y separa entornos para mayor seguridad.**

  Estas prácticas no solo mejoran la seguridad y la gestión de puertos, sino que también facilitan la depuración y la configuración de servicios en diferentes entornos.
