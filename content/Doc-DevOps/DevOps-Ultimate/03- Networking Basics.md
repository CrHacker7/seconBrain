#### NETWORKING

ip link -> TO LIST AND MODIFIY INTERFACES ON THE HOST IP
ip addr -> IS TO SEE THE IP ADDRESSES ASSIGNED TO THOSE INTERFACES
ip addr add 192.168.1.0/24 dev eth0 -> IS USED TO SET IP ADDRESSES ON THE INTERFACES
ESTOS CMDS SOLO SON VALIDOS HASTA UN RESTART, 
PARA HACERLOS DEFINITIVOS HAY QUE MODIFICAR EL FILE /etc/sysctl.conf

#### ROUTER
`ip route add 192.168.1.0/24 via 192.168.2.2` -> TO ADD ENTRIES INTO THE ROUTING TABLE
`route`  || `ip route` -> IS USED TO VIEW THE ROUTING TABLE 

**TENEMOS 3 HOSTS, HOST B ACTS AS A ROUTER**
**A,B,C**
A -> 192.168.1.5
	192.168.1.0
B -> 192.168.1.6 | 192.168.2.6
	192.168.2.0
C -> 192.168.2.5
DESDE A QUIERE COMUNICAR A C 
`ping 192.168.2.5`  //NETWORK IS UNREACHABLE

**EN HOST A** -> ip route add 192.168.2.0/24 via 192.168.1.6
> primero a qué red salto y mediante mi compañera


**EN HOST C** -> ip route add 192.168.1.0/24 via 192.168.2.6
>ASÍ, SE VEN AMBAS PARTES, AUNQUE POR DEFECTO NO, POR RAZONES DE SECURITY

**CHECK IF IP FORWARDING IS ENABLED ON A HOST IF WE´RE WORKING WITH A HOST CONFIGURED AS A ROUTER**
cat /proc/sys/net/ipv4/ip_forward -> default 0 
MODIFICAR ESTE FILE
echo 1 > /proc/sys/net/ipv4/ip_forward // 1 

Y MODIFICAR ESTE FILE
/etc/sysctl.conf
...
net.ipv4.ip_forward = 1
...

**CAMBIA DE HOST A OTRO**
`ssh app01`

Añade una ruta ip 
`sudo ip addr add 172.238.16.15/24 dev eth0`

### DNS
**Para configurar un dns** 
` cat >> /etc/hosts `
192.168.1.11           db

>La máquina que tenga esta modificación en su archivo /etc será la única a poder verlo.
Puede tener cualquier nombre que queramos, aunque ya esté cogido, como google.com.

#### Apuntar nuestro host a un servidor DNS
1. DNS Server 192.168.1.100
2. Modificar archivo en cada host
	`cat /etc/resolv.conf`
		`nameserver 192.168.1.100`

>Si la ip de un host cambiara, basta con actualizar el servidor DNS y todos los hosts deberían resolver la nueva IP

>Si tenemos otro servidor dns de pruebas se puede configurar en el archivo /etc del host. Entonces leerá primero lo que tenga en su /etc y luego el del servidor dns.

>El orden se puede cambiar en el file
>`cat /etc/nsswitch.conf`
> 
	...
	hosts:  files dns
	...
**Resolución de dominio** es la tradución de nombre de dominio a una IP 

www =>  subdominio (ayudan a reagrupar las cosas)
	maps | mail | drive | apps => 
google
.com => Top Level Domain Name
. => Root

**Servidor DNS**
192.168.1.10  => web.mycompany.com
192.168.1.11  => db.mycompany.com
192.168.1.12  => nfs.mycompany.com
192.168.1.13  => web-1.mycompany.com
192.168.1.14  => sql.mycompany.com

Si hacemos `ping web` de un host cualquiera, nos dará fallo.
Si hacemos `ping web.mycompany.com`lo resolverá.

#### Search Domain
>Para no tener que escribir el dominio completo configuramos el fichero
	cat >> /etc/resolv.conf
	   nameserver 192.168.1.100
	   search   mycompany.com  prod.mycompany.com =>podemos ponerle varios
#### Cómo se almacenan los registros en el servidor DNS?
#### Record Types
|  A    |  WEB-SERVER   | 192.168.1.1   |   
| --- | --- | --- |
|  AAA   |  WEB-SERVER   |2001:0db8:85a3:0000:0000:8a2e:0370:7334  |   
| CNAME     |    food.web-server  |  eat.web-server, hungry.web-server  |   


#### nslookup
Es una herramienta para consultar un nombre de host desde un servidor DNS
` nslookup www.google.com `
>Esta busqueda no tiene en cuenta las entradas en el archivo host /etc
#### dig
Herramienta para probar la resolución de nombres, devuelve más detalles a como se almacenane ne el servidor
` dig www.google.com `
>DNS => resolve ip on a domain name