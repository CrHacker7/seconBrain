> [!tip] Creamos el usuario postgres y database dvdrental porque así lo pone el el backup restore.sql
> Modificar (\$\$PATH\$\$) y comentar (stdin) en el archivo restore.sql
# 1. Crear contenedores y red
1. Conectar un contenedor a otro
	`docker network connect dvd-net postgres-container`
2. Levantar contenedor **postgress**
	 `docker run -d --name postgres-container -v /var/lib/postgresql/data:/var/lib/postgresql/data -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=secret -e POSTGRESS_DB=dvdrental -p 5432:5432 postgres`
3. Levantar un contenedor **pgadmin**
	 `docker run -d --name pgadmin-container -e PGADMIN_DEFAULT_EMAIL=admin@admin.com -e PGADMIN_DEFAULT_PASSWORD=admin -p 8080:80 --network \<NAME> dpage/pgadmin4`
4. Crear una red 
	 `docker network create dvd-net`
5. Añadir contenedores a la red
	 `docker network connect dvd-net postgres-box`
    `docker network connect dvd-net pgadmin-box`
	     1. *docker network connect \<red> \<container>*
6. Inpeccionar una red
	- `docker network inspect dvd-net` 

# 2.Datos para agregar el servidor en pgAdmin:
1. ADD NEW SERVER
2. NAME: mypostgres
3. HOST NAME/ADDRESS: 172.18.0.2 (ip postgres)
4. PORT: 5432 (Este es el puerto por defecto de PostgreSQL).
5. MAINTENANCE DATABASE: dvdrental (variable POSTGRESS_DB) 
6. USERNAME: postgres (variable POSTGRES_USER).
7. PASSWORD: postgres (variable POSTGRES_PASSWORD).

**Para ver la IP de los contenedores**
- `docker inspect dvd-net`
# 3. Copiar y Extraer archivo .tar 
###### Fuera del contenedor:
- `docker cp /home/Escritorio/postgres/dvdrental.tar postgres-container:/tmp/`
	- *docker cp <ruta_local> <nombre_del_contenedor>:<ruta_dentro_del_contenedor>*
###### Restaurar el archivo .tar dentro de la shell de PostgreSQL:
- `docker exec -it postgres-box bash`
- `su - postgres`
###### Verificar el archivo de backup, para saber si el .tar es un dump de PostgreSQL válido, 
- `tar -tvf /tmp/db/dvdrental.tar`
###### Verificar propiedad de un directorio los permisos del directorio de datos de PostgreSQL en el contenedor 
- `ls -l /var/lib/postgresql/data`
##### Darle permisos para la extracción y restablece la propiedad de los archivos al usuario postgres
- `tar -tvf /tmp/dvdrental.tar`
	- verifica permisos del archivo .tar
- `chown postgres:postgres /tmp/dvdrental.tar`
- `chmod 700 /ruta/a/restore.sql`
- `chown -R postgres:postgres /tmp/`
- `chmod -R 700 /tmp/`
- *`chown -R postgres:postgres /var/lib/postgresql/data`* (volumen)
- *`chmod -R 700 /var/lib/postgresql/data`* (volumen)
###### Qué permisos son necesarios para extraer el .tar
1. Permisos de ***lectura*** en el archivo ***.tar*,** es decir, que el usuario que va a ejecutar el comando tar debe poder leer el archivo .tar.
2. Permisos de ***escritura*** en el ***directorio destino*** donde se van a extraer los archivos (no en el archivo .tar en sí, sino en el **directorio destino**). El usuario debe tener permiso de escritura para colocar los archivos extraídos en el directorio de destino.
- `tar -xvf /tmp/dvdrental.tar -C /tmp/`
	- *tar -xvf /tmp/dvdrental.tar -C /home/usuario/extracciones*
	- -C /ruta/donde/extraer: La opción -C indica el directorio donde quieres que se extraigan los archivos. 
-  `tar -xvf /tmp/dvdrental.tar`
	- /var/lib/postgresql (extrae en el directorio actual '/' )

> [!warning] SI nada funciona, hacerlo con el root

> [!tip] 'postgres' vs 'psql -U postgres'
> **su - postgres:** Cambias al usuario postgres del sistema operativo. Luego puedes ejecutar comandos de PostgreSQL, archivos de configuración, o realizar otras tareas a nivel de sistema.
**psql -U postgres:** Te conecta a la base de datos PostgreSQL como el usuario postgres, sin cambiar de usuario en el sistema operativo.


> [!tip] '$' vs '#'
> ***postgres@377c69c8071d:~$:*** Estás en la terminal del sistema operativo como el usuario postgres.
> ***postgres=#:*** Estás en la interfaz de psql, interactuando con la base de datos PostgreSQL como el superusuario postgres. # indica que tienes privilegios de superusuario dentro de PostgreSQL. 
> -  ~ indica que estás en el directorio home del usuario postgres dentro del sistema operativo.
> - $ es el indicador de que estás trabajando como un usuario normal (en contraste con el prompt de root, que sería #).
>- Si fueras un usuario normal en PostgreSQL, el prompt sería postgres=> (sin el #).

###### Ejecuta el siguiente comando para extraer el archivo .tar:
- `tar -xvf /tmp/dvdrental.tar -C /tmp`
	- *tar -xvf /tmp/dvdrental.tar -C /home/usuario/extracciones*

# Cuando hayamos copiado todos los ficheros dentro del contenedor

##### 1 Verificar que la base de datos dvdrental existe en PostgreSQL
- psql -U postgres -l
##### 2️ Crear la base de datos manualmente
Si dvdrental no está en la lista, créala con este comando:
- psql -U postgres -c "CREATE DATABASE dvdrental;"
Después, confirma que existe:
- psql -U postgres -l
Si la base de datos dvdrental ahora aparece en la lista, entonces ya está lista para usarse.
##### 3. Ejecutar el archivo restore.sql
- `psql -U postgres -d dvdrental -f /tmp/restore.sql`
##### 4 . Verificar la restauración
- `psql -U postgres -d dvdrental -c "\dt"`
Y para ver los primeros registros de una tabla, usa algo como:
- `psql -U postgres -d dvdrental -c "SELECT * FROM nombre_de_tu_tabla LIMIT 5;"`
	- *psql -U postgres -d nombre_db -f /ruta/del/archivo.sql*
###### Reiniciar contenedor
- docker restart postgres-container

###### Eliminar directorio dentro del contenedor
LIstamos lo que tenemos
- ls -la /tmp
Eliminamos
- rm -rf /tmp
Si eliminas /tmp, es posible que algunos programas o servicios del contenedor no funcionen correctamente, ya que /tmp es un directorio utilizado para almacenar archivos temporales.
Si solo deseas limpiar el contenido de /tmp y no eliminar el directorio en sí, puedes usar:
- `rm -rf /tmp/*`

 Si el archivo no tiene toc.dat, entonces no es un dump compatible con pg_restore (binario), lo que explicaría el error.
`pg_restore -U postgres -d dvdrental /tmp/archivo.tar`
Esto restaurará el archivo .tar en la base de datos dvdrental.

##### Si no tienes la base de datos dvdrental creada previamente, puedes agregar el parámetro -C (para crearla):
#### Cuando hayamos copiado todos los ficheros dentro del contenedor

##### 1. Verificar que la base de datos dvdrental existe en PostgreSQL
- psql -U postgres -l
##### 2️ Crear la base de datos manualmente
Si dvdrental no está en la lista, créala con este comando:
- psql -U postgres  -c "CREATE DATABASE dvdrental;"
Después, confirma que existe:
- psql -U postgres -l
##### 3. Ejecutar el archivo restore.sql
- psql -U postgres -d dvdrental -f /tmp/restore.sql
##### 4 . Verificar la restauración
- psql -U postgres -d dvdrental -c "\dt"
Y para ver los primeros registros de una tabla, usa algo como:
- psql -U postgres -d dvdrental -c "SELECT * FROM nombre_de_tu_tabla LIMIT 5;"

pg_restore -U postgres -C -d postgres /tmp/archivo.tar
Explicación:

-U postgres: El nombre de usuario para conectarse a PostgreSQL.
-d dvdrental: El nombre de la base de datos donde se restaurarán los datos.
/tmp/archivo.tar: La ubicación del archivo .tar dentro del contenedor.
-C: Si la base de datos no existe, crea la base de datos.
Verificar la restauración:

Una vez que se haya ejecutado el comando de restauración, puedes verificar que la restauración se haya realizado correctamente de dos maneras:

Desde la terminal dentro del contenedor: Ejecutando el comando psql:

psql -U postgres -d dvdrental
Desde pgAdmin: Ahora que la base de datos está restaurada, debes poder verla en pgAdmin bajo la conexión a PostgreSQL. Solo asegúrate de refrescar la vista de bases de datos en pgAdmin.

##### Confirmar la importación
Después de restaurar, asegúrate de que las tablas existen:
- psql -U postgres -d dvdrental -c "\dt"

##### ¿Por qué usar el archivo .tar?
El archivo .tar es más fácil de manejar porque:

No necesitas preocuparte por editar rutas o modificar archivos de datos manualmente.
El archivo .tar ya tiene la estructura y los datos que necesitas restaurar, lo que hace que el proceso de restauración sea más directo.

1. Copia el archivo .tar al contenedor Docker.
2. Accede al contenedor Docker.
3. Usa el comando psql para restaurar
4. Verifica la restauración en pgAdmin o usando psql en la terminal.



Ejecuta psql como el usuario postgres sin usar sudo:
- su - postgres
- psql -U postgres -d dvdrental
Entrar en contenedor
- docker exec -it postgres-container su - postgres
- psql -U postgres -d dvdrental

### Como root dentro del contenedor postgres-container
- **docker exec -it postgres-container bash (estamos dentro)**
- **apt update**
- **apt install sudo**
- **apt install postgresql-client**

---
Si necesitas configurar el contenedor o gestionar aspectos del sistema, es mejor usar el usuario root.
Si necesitas gestionar la base de datos en PostgreSQL, como crear bases de datos o modificar la configuración de PostgreSQL, usa postgres.
Si solo necesitas acceder a datos o realizar tareas limitadas dentro de la base de datos, como hacer consultas o manipulaciones, pero no quieres otorgar privilegios de administrador, usa un usuario como postgres. 






> [!failure] verificar si es correcto hacer esto o NO!
> ### Añadiendo directorio de películas a Volumen
> No me permite modificar porque no soy Owner, 
> así que cambiamos a:
> - **sudo chown -R mooc:mooc /var/lib/postgresql/data**
> - Ahora puedo insertar el dir dvdrental.tar , para jugar con él.


##
root@bcefd6a1f76f:/# chmod -R 700 /tmp/
root@bcefd6a1f76f:/# ls -l /tmp/
total 5568
-rwx------ 1 postgres postgres   57147 May 12  2019 3055.dat
-rwx------ 1 postgres postgres    8004 May 12  2019 3057.dat
-rwx------ 1 postgres postgres     483 May 12  2019 3059.dat
-rwx------ 1 postgres postgres  333094 May 12  2019 3061.dat
-rwx------ 1 postgres postgres  149469 May 12  2019 3062.dat
-rwx------ 1 postgres postgres   26321 May 12  2019 3063.dat
-rwx------ 1 postgres postgres   46786 May 12  2019 3065.dat
-rwx------ 1 postgres postgres   21762 May 12  2019 3067.dat
-rwx------ 1 postgres postgres    3596 May 12  2019 3069.dat
-rwx------ 1 postgres postgres  140422 May 12  2019 3071.dat
-rwx------ 1 postgres postgres     263 May 12  2019 3073.dat
-rwx------ 1 postgres postgres  718644 May 12  2019 3075.dat
-rwx------ 1 postgres postgres 1214420 May 12  2019 3077.dat
-rwx------ 1 postgres postgres     271 May 12  2019 3079.dat
-rwx------ 1 postgres postgres      57 May 12  2019 3081.dat
-rwx------ 1 postgres postgres 2835456 Feb 19 10:33 dvdrental.tar
-rwx------ 1 postgres postgres   45857 Feb 19 10:33 restore.sql
-rwx------ 1 postgres postgres   55111 May 12  2019 toc.dat
root@bcefd6a1f76f:/# tar -tvf /tmp/dvdrental.tar
-rw------- postgres/postgres 55111 2019-05-12 03:36 toc.dat
-rw------- postgres/postgres  8004 2019-05-12 03:36 3057.dat
-rw------- postgres/postgres 46786 2019-05-12 03:36 3065.dat
-rw------- postgres/postgres   483 2019-05-12 03:36 3059.dat
-rw------- postgres/postgres 21762 2019-05-12 03:36 3067.dat
-rw------- postgres/postgres  3596 2019-05-12 03:36 3069.dat
-rw------- postgres/postgres 57147 2019-05-12 03:36 3055.dat
-rw------- postgres/postgres 333094 2019-05-12 03:36 3061.dat
-rw------- postgres/postgres 149469 2019-05-12 03:36 3062.dat
-rw------- postgres/postgres  26321 2019-05-12 03:36 3063.dat
-rw------- postgres/postgres 140422 2019-05-12 03:36 3071.dat
-rw------- postgres/postgres    263 2019-05-12 03:36 3073.dat
-rw------- postgres/postgres 718644 2019-05-12 03:36 3075.dat
-rw------- postgres/postgres 1214420 2019-05-12 03:36 3077.dat
-rw------- postgres/postgres     271 2019-05-12 03:36 3079.dat
-rw------- postgres/postgres      57 2019-05-12 03:36 3081.dat
-rw------- mooc/mooc           45857 2025-02-19 10:33 restore.sql
root@bcefd6a1f76f:/# 



# Probar esto
Sin embargo, si deseas hacer todo desde pgAdmin, puedes crear la base de datos manualmente (clic derecho en "Databases" > "Create" > "Database...") y luego restaurarla usando la opción "Restore".
El orden correcto sería:
Crear la base de datos en pgAdmin:

Abre pgAdmin.
Haz clic derecho sobre "Databases" y selecciona Create > Database....
Nombra la base de datos como dvdrental (o el nombre que corresponda).
Crea la base de datos.
Restaurar la base de datos:

Una vez que la base de datos esté creada, haz clic derecho sobre la base de datos dvdrental en pgAdmin.
Selecciona Restore.
Elige el archivo restore.sql como archivo de restauración.
Haz clic en Restore.