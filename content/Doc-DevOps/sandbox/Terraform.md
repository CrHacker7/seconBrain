Pasos para usar Terraform con VirtualBox y poner en marcha tu máquina virtual
1. Instalar Terraform
Primero, asegúrate de tener Terraform instalado en tu máquina. Si no lo tienes, puedes descargarlo desde su página oficial:

Descargar Terraform
2. Instalar VirtualBox
Para que Terraform pueda crear una máquina virtual en tu computadora, necesitas tener VirtualBox instalado. Puedes descargarlo aquí:

Descargar VirtualBox
3. Instalar el proveedor de VirtualBox para Terraform
El proveedor de VirtualBox es lo que permite que Terraform interactúe con VirtualBox y cree las máquinas virtuales. Si no lo tienes configurado, Terraform lo descargará automáticamente cuando ejecutes el comando, pero asegúrate de tener VirtualBox instalado antes.

4. Escribir y preparar el código de Terraform
El código que te proporcioné debe estar guardado en un archivo de configuración .tf. Puedes crear este archivo en un directorio de tu elección. Por ejemplo, crea un archivo llamado main.tf con el siguiente código:
```java
provider "virtualbox" {}

resource "virtualbox_vm" "ubuntu_vm" {
  name   = "Ubuntu_VM"
  image  = "ubuntu_20.04_64"
  cpu    = 2
  memory = 4096

  network_adapter {
    type = "bridged"
  }

  disk {
    size = 10240
  }
}
```
Detalles a revisar:

image: Este es el nombre de la imagen del sistema operativo que deseas usar. Asegúrate de tener una imagen de Ubuntu 20.04 o la que prefieras descargada en tu VirtualBox. Si no tienes la imagen, necesitarás crearla o descargarla.
cpu y memory: Estos son los recursos de la máquina virtual, puedes ajustarlos según lo que quieras (más CPU o más memoria si lo necesitas).
network_adapter: El tipo de red que usarás (en este caso, "bridged" permite que la máquina virtual tenga su propia IP en la red local).
disk: Tamaño del disco duro virtual de la máquina.
5. Inicializar Terraform
Una vez que tengas tu archivo .tf listo, abre la terminal o línea de comandos, navega hasta el directorio donde tienes el archivo main.tf, y ejecuta el siguiente comando para inicializar el proyecto:

bash
Copiar código
`terraform init`
Este comando descarga los plugins necesarios, como el proveedor de VirtualBox.

6. Ver el plan de ejecución
Antes de que Terraform cree la máquina virtual, es recomendable ver qué cambios va a realizar. Para esto, ejecuta:
`terraform plan`
Terraform te mostrará un resumen de lo que va a crear, como una máquina virtual, discos, etc.

7. Aplicar los cambios
Si todo parece bien, puedes proceder a aplicar los cambios. Esto creará la máquina virtual según las instrucciones que definiste:
`terraform apply`
Te pedirá confirmación. Escribe yes para continuar.

8. Acceder a la máquina virtual
Una vez que Terraform haya creado la máquina virtual, puedes acceder a ella. VirtualBox debería haber lanzado la VM, y podrás verla desde la interfaz gráfica de VirtualBox. Si configuraste una red "bridged", la máquina virtual tendrá su propia IP y podrás acceder a ella por SSH si tienes habilitado el servicio SSH en la VM.

9. Configurar la máquina virtual (opcional)
Si necesitas que la máquina virtual haga algo más (como instalar software automáticamente), puedes usar un enfoque como el de provisionamiento de Terraform. Esto te permite ejecutar scripts automáticamente cuando la VM se crea. Aquí hay un ejemplo para instalar Apache en la máquina virtual:
```java
resource "virtualbox_vm" "ubuntu_vm" {
  name   = "Ubuntu_VM"
  image  = "ubuntu_20.04_64"
  cpu    = 2
  memory = 4096

  network_adapter {
    type = "bridged"
  }

  disk {
    size = 10240
  }

  provisioner "remote-exec" {
    inline = [
      "sudo apt update",
      "sudo apt install -y apache2"
    ]
  }
}
```

Este ejemplo usa un provisioner para ejecutar comandos directamente en la máquina virtual después de que se haya creado. Con eso, Apache se instalará automáticamente en tu máquina virtual.

¿Qué más necesitas?
Acceder al servidor de la máquina virtual: Si la máquina tiene acceso a la red, puedes acceder a ella por SSH utilizando la IP que se le haya asignado (si está en red bridged) o utilizando la interfaz de VirtualBox si la conectividad no está configurada por red.

Instalar software adicional: Puedes usar los provisionadores de Terraform (como remote-exec) para instalar y configurar el software necesario, o puedes configurarlo manualmente después de que la VM esté en funcionamiento.

Automatización completa: Terraform se encarga de crear la infraestructura (en este caso, la máquina virtual), pero si necesitas un entorno de desarrollo o una configuración más avanzada, tendrás que añadir más recursos y configuraciones (como redes, almacenamiento, etc.) según lo que necesites.
***
Resumen de pasos:
Prepara tu entorno: Instala Terraform y VirtualBox.
Escribe el archivo de configuración .tf.
Ejecuta los comandos de Terraform:
terraform init para inicializar.
terraform plan para ver el plan.
terraform apply para aplicar el plan y crear la VM.
Accede a la máquina virtual desde la interfaz de VirtualBox o mediante SSH.

## Configuración Avanzada para desarrolladores
```java
provider "virtualbox" {}

# Crear la máquina virtual Ubuntu
resource "virtualbox_vm" "dev_vm" {
  name   = "Dev_VM"
  image  = "ubuntu_20.04_64"  # Asegúrate de tener la imagen descargada
  cpu    = 2
  memory = 4096

  network_adapter {
    type = "bridged"  # Usa red bridged para que tenga una IP accesible en la red local
  }

  disk {
    size = 10240  # Tamaño del disco en MB
  }

  # Provisión inicial para instalar herramientas de desarrollo
  provisioner "remote-exec" {
    inline = [
      "sudo apt update",
      "sudo apt install -y git docker.io nodejs npm build-essential curl",
      "sudo systemctl enable docker",  # Habilitar Docker al inicio
      "sudo systemctl start docker",   # Iniciar Docker
      "curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -",  # Instalar Node.js
      "sudo apt-get install -y nodejs",
      "git config --global user.name 'Tu Nombre'",
      "git config --global user.email 'tu_email@dominio.com'"
    ]
  }

  # Provisión de SSH (esto es opcional pero útil si deseas acceder remotamente)
  provisioner "file" {
    source      = "id_rsa"  # Tu clave privada SSH
    destination = "/home/ubuntu/.ssh/id_rsa"
  }

  provisioner "remote-exec" {
    inline = [
      "chmod 600 /home/ubuntu/.ssh/id_rsa",  # Asegúrate de que la clave SSH tenga los permisos correctos
      "sudo mkdir -p /home/ubuntu/.ssh"
    ]
  }
}


```
Descripción de lo que hace el código:
virtualbox_vm "dev_vm": Este recurso crea una máquina virtual con Ubuntu 20.04, configurando 2 CPUs, 4GB de memoria y un disco de 10GB.

network_adapter { type = "bridged" }: Esto hace que la máquina virtual esté en la misma red que tu máquina local, por lo que tendrá una IP accesible desde otros dispositivos en la red local.

Provisión de software de desarrollo:

Instala Git, Docker, Node.js, NPM y herramientas de compilación (como build-essential).
Configura Docker para que inicie automáticamente con el sistema.
Establece tu nombre y correo electrónico para Git.
provisioner "remote-exec": Ejecuta comandos dentro de la máquina virtual después de que se haya creado para configurar el entorno de desarrollo.

provisioner "file": Copia tu clave SSH a la máquina virtual para que puedas acceder a ella de forma remota usando SSH (esto es útil si prefieres no usar la interfaz gráfica de VirtualBox).

Permisos de la clave SSH: Establece los permisos correctos para la clave SSH copiada, permitiendo el acceso remoto de forma segura.

2. Iniciar el Proceso con Terraform
Una vez que tengas el archivo main.tf con el código anterior, sigue estos pasos para ponerlo en marcha:

Inicializa el directorio de trabajo de Terraform:

Navega a la carpeta donde tienes el archivo main.tf y ejecuta el siguiente comando en la terminal para inicializar el proyecto:
`terraform init`
Esto descargará los plugins necesarios para que Terraform interactúe con VirtualBox.

Verifica los cambios con terraform plan:

Antes de aplicar los cambios, ejecuta el siguiente comando para asegurarte de que Terraform sabe qué cambios va a hacer:
`terraform plan`
Esto te mostrará lo que va a crear: la máquina virtual, los discos, las redes, etc. Asegúrate de que todo se ve bien.

Aplica los cambios con terraform apply:

Si todo está en orden, ejecuta:
`terraform apply`
3. Acceder a la Máquina Virtual
Una vez que la máquina virtual se haya creado, puedes acceder a ella de las siguientes maneras:

Por la interfaz de VirtualBox:

Si usas una red bridged, la máquina virtual tendrá una IP local (como 192.168.x.x), que puedes encontrar en la interfaz de VirtualBox. Solo tienes que ir a la sección de "Red" en VirtualBox y buscar la dirección IP de la VM.
Si la máquina no tiene acceso SSH aún, puedes usar la interfaz gráfica de VirtualBox para abrir la consola de la VM directamente.
Acceso por SSH (si usaste el provisionador de SSH):

Si configuraste el SSH, puedes acceder a la máquina usando una terminal y el siguiente comando:
`ssh -i /path/to/your/private_key ubuntu@<ip_de_la_vm>`

Asegúrate de reemplazar your/private_key con la ruta de tu clave SSH y <ip_de_la_vm> con la dirección IP que la máquina virtual tiene en la red.
4. Instalar y Configurar Herramientas Adicionales (Opcional)
Si necesitas más configuraciones específicas para tu entorno de desarrollo, como bases de datos, servidores web o lenguajes adicionales, puedes modificar el código de Terraform para incluir más provisionadores o scripts. Aquí algunos ejemplos:

Instalar PostgreSQL:
`"sudo apt-get install -y postgresql postgresql-contrib"`