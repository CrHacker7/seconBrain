SALE LA SHELL QUE USAMOS
`echo $SHELL`

HACE 3 CMD EN UNA VEZ
`cd my_directory; mkdir new_folder; pwd`
home/my_directory/new_folder

HACE UNA JERARQUÍA DE DIRECTORIOS (-p = position jeráquica hehe)
`mkdir -p /tmp/asia/india/bangalore`

ELIMINAR UN DIRECTORIO
`rm -r /tmp/my_directory`

COPIAR DIRECTORIO COMPLETO
`cp -r my_directory /tmp/my_directory`

CREAR NUEVO FILE SIN CONTENIDO
`touch new_file.txt`
touch /home/thor/empty_file.txt

AÑADIR CONTENIDO AL FILE
`cat > new_file.txt`
CTRL + D (mayúsculas) save

VISUALIZAR CONTENIDO DEL FILE
`cat new_file.txt`

COPIAR FILE
`cp new_file.txt copy_file.txt`

COPIAR FILE A DIRECTORIO
`cp -v /home/thor/asia/bangalore.txt /home/thor/asia/india/bagalore`

COPIAR SOURCE DIRECTORY TO TARGET LOCATION
`cp -r /home/thor/asia/india/bangalore /home/thor/`

Remove target file from target directory
rm /home/thor/asia/bangalore.txt

MOVER O RENOMBRAR FILE
`mv new_file.txt sample_file.txt`

ELIMINAR UN FILE
`rm new_file.txt`
____________________________________________________________________________
#### VI EDITOR
PARA EDITAR CON VI ESTAMOS EN "COMMAND MODE" POR DEFECTO, 
HAY QUE TIPEAR ==<i>== PARA "INSERT MODE"
Y PODER EDITARLO Y PARA VOLVER A "COMMAND MODE" > ESC

SAVE
```bash
:w
```
```bash
:w filename
```
QUIT (DISCARD)
```bash
`:q`
```
SAVE + QUIT
```bash
`:wq
```
FIND A WORD (of)
```bash
`/of`   
`n` (para mover entre todas las opciones de of)
```
USER ACCOUNTS
```bash
whoami
 carly
 ```
```bash
`id`
uid=1000(carly) gid=1000(carly) groups=1000(carly),999(docker)
```
CAMBIA DE UN USER A OTRO
`su <aparna>`

CAMBIA DE USER MEDIANTE SSH y logearse en él
`ssh aparna@192.168.1.2`

GUARDADO LOCAL DEL FILE AÑADIENDO "MENOS O DE OSO"
`curl http://www.some-site.com/some-file.txt -O`
`wget http://www.some-site.com/some-file.txt -O some-file.txt`

CHECK OS VERSION
`ls /etc/*release*`   //    /etc/lsb-release  /etc/os-release

>sale toda la info
>`cat /etc/*release*` 
________________________________________________________________________
#### RPM (RED HAT PACKAGE MANAGER)

INSTALL PACKAGE -> `rpm -i telnet.rpm`
UNINSTALL PACKAGE -> `rpm -e telnet`
INSTALA MEDIANTE QUERY PACKAGE -> `rpm -q telnet` 

ELIMINA UN PAQ. INSTALADO
`sudo rpm -e ftp`
>-e de erase))

INSTALA A FTP CON RPM A DIRECTORY /OPT/
`sudo rpm -i /opt/ftp-0.17-89.el9.x86_64.rpm`

DA UNA LISTA ENORME DE TODOS LOS PAQUETES QUE HAY INSTALADOS
`rpm -qa`

BUSCA EN MEDIO DE TODOS LOS PAQUETES QUE TENGO
`rpm -qa | grep ftp`
_____________________________________________________________________
#### YUM REPOS

DA UNA LISTA DE TODOS LOS REPOS DE CENTOS
`yum repolist`

**BUSCA SI TENEMOS INSTALADOS ESTOS 4 SOFTWARESCL**
`rpm -q ansible python3 telnet openssh-server`
 telnet-0.17-85.el9.x86_64
 openssh-server-8.7p1-43.el9.x86_64
 package ansible is not installed
 python3-3.9.19-8.el9.x86_64

LISTA DÓNDE ESTÁN CONFIGURADOS LOS FILES DE LOS REPOS
`ls /etc/yum.repos.d/`  (terminan en .repo)

MUESTRA EL SITIO DÓNDE ESTÁN ALMACENADOS
`cat /etc/yum.repos.d/CentOS-Base.repo`

BUSCAR UN PAQUETE PARTICULAR (name, version, installed or not)
`yum list ansible`

REMOVE A PACKAGE
`sudo yum remove ansible`

MUESTRA LOS DUPLICADOS
`yum --showduplicates list ansible`

PARA INSTALAR UN PAQUETE ESPECIFICO SI TENEMOS DOS (añade < - > y <version>)
`sudo yum install ansible-2.4.2.0`
`sudo yum install -y maven (instala maven)`
_____________________________________________________________________________
#### SERVICES

INICIA UN SERVICIOAPACHE WEB SERVER QUE
`service httpd start`

INICIA CON EL KERNEL ES LO MISMO QUE CON EL APACHE WEB
`systemctl start httpd`

STOP HTTPD SERVICE
`systemctl stop httpd`

CHECK STATUS
`systemctl status httpd`

CONFIGURA HTTPD TO START AT STARTUP
`systemctl enable httpd`

**CONFIGURA HTTPD TO NOT START AT STARTUP**
`systemctl disable httpd`

**PARA CONFIGURAR APP PARA ARRANCAR COMO SERVICIO**
DENTRO DE ESTA RUTA-> /etc/systemd/system
creamos file my_app.service
AÑADIMOS
[Service] 
ExecStart=/usr/bin/python3 /opt/code/my_app.py
(optional)
ExecStart=/opt/code/configure_db.sh
ExecStart=/opt/code/email_status.sh
Restart=always (reinicia siempre después de it crashes)

REFRESCAMOS -> systemctl daemon-reload
REINICIAMOS -> systemctl start my_app
VER EL STATUS SI ESTA ACTIVO ->systemctl status my_app
SE TESTEA -> curl http://localhost:5000

**PARA ARRANCARLO DESPUÉS DE OTRO**
DETRO DEL FILE my_app.service
AÑADIMOS
[Install]
WantedBy=multi-user.target
PARA BOOT UP, 
AÑADIMOS
[Unit]
Description=My python web app

**VERIFICA LA CONFIG DEL FILE**
`systemctl cat app.service`
`cat /usr/lib/systemd/system/app.service` (otra manera)

___________________________________________________________
NETWORKING

ip link -> TO LIST AND MODIFIY INTERFACES ON THE HOST IP
ip addr -> IS TO SEE THE IP ADDRESSES ASSIGNED TO THOSE INTERFACES
ip addr add 192.168.1.0/24 dev eth0 -> IS USED TO SET IP ADDRESSES ON THE INTERFACES
ESTOS CMDS SOLO SON VALIDOS HASTA UN RESTART, 
PARA HACERLOS DEFINITIVOS HAY QUE MODIFICAR EL FILE /etc/sysctl.conf

ROUTER
ip route add 192.168.1.0/24 via 192.168.2.2 -> TO ADD ENTRIES INTO THE ROUTING TABLE
route  || ip route -> IS USED TO VIEW THE ROUTING TABLE 

TENEMOS 3 HOSTS, HOST B ACTS AS A ROUTER
A,B,C
A -> 192.168.1.5
	192.168.1.0
B -> 192.168.1.6 | 192.168.2.6
	192.168.2.0
C -> 192.168.2.5
DESDE A QUIERE COMUNICAR A C 
ping 192.168.2.5 //NETWORK IS UNREACHABLE

EN HOST A -> ip route add 192.168.2.0/24 via 192.168.1.6
EN HOST C -> ip route add 192.168.1.0/24 via 192.168.2.6
ASÍ, SE VEN AMBAS PARTES, AUNQUE POR DEFECTO NO, POR RAZONES DE SECURITY

MODIFICAR ESTE FILE
CHECK IF IP FORWARDING IS ENABLED ON A HOST IF WE´RE WORKING WITH A HOST CONFIGURED AS A ROUTER
cat /proc/sys/net/ipv4/ip_forward -> default 0 
MODIFICAR ESTE FILE
echo 1 > /proc/sys/net/ipv4/ip_forward // 1 

Y MODIFICAR ESTE FILE
/etc/sysctl.conf
...
net.ipv4.ip_forward = 1
...

CAMBIA DE HOST A OTRO
ssh app01
___________________________________________________________
