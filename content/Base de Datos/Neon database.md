##### Copiar el archivo dentro del contenedor
- $ docker cp /home/mooc/Escritorio/postgres/dvdrental.tar postgres-container:/tmp/dvdrental.tar
##### Entrar dentro del contenedor como usuario Postgres/admin
- **docker exec -it postgres-container psql -U postgres**
- **docker exec -it postgres-container psql -U admin
**
##### Darle privilegios elevados a mi usuario.
- ALTER USER <nombre_de_usuario> WITH SUPERUSER;
##### Darle privilegios a mi usuario admin .
- GRANT ALL PRIVILEGES ON DATABASE dvdrental TO admin;

#### 1️ Dentro de postgres, verificar el estado del contenedor y la base de datos 
- psql -U admin -l
##### Acceso al sistema de archivos completo del contenedor, inspeccionar archivos, editar configuraciones, instalar nuevas herramientas, etc
- docker exec -it postgres-container /bin/bash
##### Dentro del contenedor con psql utilizando el cliente admin, interactuar directamente con la base de datos
- docker exec -it postgres-container psql -U admin

##### 2️ Crear la base de datos manualmente
- psql -U admin -c "CREATE DATABASE dvdrental;"
Después, confirma que existe:
- psql -U admin -l

> [!warning] NO SIMBOLOS
>NO ACEPTA GUIONES EN EL NOMBRE

##### Crear usuario  y password
- CREATE USER test WITH PASSWORD 'test';

##### 3️ Verificar el archivo de backup

Para saber si el .tar es un dump de PostgreSQL válido, ejecuta este comando en el host o en el contenedor donde descargaste el archivo:

- tar -tf /tmp/dvdrental.tar

Si el archivo no tiene toc.dat, entonces no es un dump compatible con pg_restore (binario), lo que explicaría el error.
##### 4 Extraer e importar los datos
Si el .tar contiene un restore.sql
Si dentro del .tar tienes un restore.sql, entonces usa psql para importarlo después de crear la base de datos:
- Cambiamos el usuario a POSTGRES para extraer el .tar
- **tar -xvf /tmp/dvdrental.tar -C /tmp/**
verificar si tenemos la base de datos
- psql -U postgres -l  
	- Este comando lista todas las bases de datos. Verifica si dvdrental está en la lista. Si no está, necesitarás crearla antes de intentar restaurarla.
- psql -U postgres
	- Este comando te conectará a PostgreSQL como el usuario postgres (suponiendo que este es el usuario de administrador).
	- Una vez conectado, puedes verificar los roles y privilegios de postgres con la siguiente consulta SQL:
- SELECT rolname, rolsuper FROM pg_roles WHERE rolname = 'postgres';

> [!NOTE] Si el usuario postgres es un superusuario
```
  rolname  | rolsuper 
-----------+----------
 postgres  | t

```
- **psql -U postgres -d dvdrental -f /tmp/restore.sql**
Si falla, revisa los permisos y la ubicación del archivo.

##### 5️ Mover el archivo dentro del contenedor
Si necesitas mover el archivo a tu contenedor, usa:
- docker cp dvdrental.tar postgres_container:/tmp/dvdrental.tar
Y dentro del contenedor, verifica:
- ls -l /tmp/dvdrental.tar
Si el archivo tiene permisos restringidos, cambia los permisos:
- chmod 644 /tmp/dvdrental.tar

##### 6️ Confirmar la importación
Después de restaurar, asegúrate de que las tablas existen:
- psql -U admin -d dvdrental -c "\dt"

#####  7 Verificar si existe el archivo SQL (restore.sql)
ls -l /tmp/

##### ¿El backup es un .tar válido?

Si tar -tf sigue fallando, el archivo podría estar corrupto o no ser un backup de PostgreSQL.

En tu máquina local, prueba:

- file dvdrental.tar
- pg_restore -l dvdrental.tar
Si file dvdrental.tar dice algo diferente a "POSIX tar archive", el archivo podría no ser un backup de PostgreSQL.

##### Eliminar la carpeta del contenedor
Dentro del contenedor
- rm -rf /tmp/dvdrental.tar
- rm 3055.dat 3057.dat 
	- rm \<nombre_archivos>
##### 1. Verificar el contenido del archivo .tar: Ejecuta el siguiente comando para ver el contenido del archivo tar sin extraerlo:
- tar -tvf /tmp/dvdrental.tar
Esto te mostrará si realmente contiene lo que esperamos (archivos y/o directorios de base de datos).

##### 2. Extraer el archivo si contiene los datos correctamente: Si todo se ve bien, puedes intentar extraerlo:
- tar -xf /tmp/dvdrental.tar -C /tmp/

#### Cuando hayamos copiado todos los ficheros dentro del contenedor

##### 1. Verificar que la base de datos dvdrental existe en PostgreSQL
- psql -U admin -l
##### 2️ Crear la base de datos manualmente
Si dvdrental no está en la lista, créala con este comando:
- psql -U admin  -c "CREATE DATABASE dvdrental;"
Después, confirma que existe:
- psql -U admin -l
##### 3. Ejecutar el archivo restore.sql
- psql -U admin -d dvdrental -f /tmp/restore.sql
##### 4 . Verificar la restauración
- psql -U admin -d dvdrental -c "\dt"
Y para ver los primeros registros de una tabla, usa algo como:
- psql -U admin -d dvdrental -c "SELECT * FROM nombre_de_tu_tabla LIMIT 5;"

#### Role postgres not exist
Verifica los usuarios en tu PostgreSQL con:
- psql -U postgres -c "\du"
Si postgres no existe, créalo con:
- psql -U postgres -c "CREATE ROLE postgres WITH LOGIN SUPERUSER PASSWORD 'tu_contraseña';"
Si prefieres usar otro usuario (admin), entonces edita restore.sql para cambiar postgres por admin:
- sed -i 's/OWNER TO postgres/OWNER TO admin/g' /tmp/restore.sql

### PGADMIN
##### Ver qué usuario usa:
- SELECT current_user;
##### Dar todos los permisos a usuario test
- GRANT ALL PRIVILEGES ON DATABASE nombre_de_base_de_datos TO test;
##### Si el usuario necesita más privilegios (como insertar, actualizar o eliminar registros), puedes usar otros privilegios:
- GRANT SELECT, INSERT, UPDATE, DELETE ON film TO test;
##### Este comando te mostrará los permisos actuales sobre la tabla y podrás verificar si el usuario tiene los permisos correctos.
- \dp film
##### Elimina la base de datos
- postgres-# psql -U test -d postgres -c "DROP DATABASE IF EXISTS dvdrental;"



##### Conéctate a postgres 
- psql -U test -d postgres

##### Create role postgres because the restore.sql
- CREATE ROLE postgres WITH LOGIN SUPERUSER PASSWORD 'postgres';
##### Base de datos con locale válido
- psql -U test -d postgres -c "CREATE DATABASE dvdrental LC_COLLATE='en_US.UTF-8' LC_CTYPE='en_US.UTF-8';"
##### Ver Conexión a base de datos y usuario
- postgres-# \c dvdrental
You are now connected to database "dvdrental" as user "test".
- postgres-# \c postgres
You are now connected to database "postgres" as user "test".

##### Si ya estás dentro de psql como otro usuario (test en tu caso), no puedes hacer su dentro de psql. Debes salir (\q) y reconectarte como postgres.

> [!warning] Cambio de user
> ==mooc@mooc-VirtualBox:$== docker exec -it postgres-container /bin/bash
> ==root@9acdf1697be2:/#== su - postgres
> ==postgres@9acdf1697be2:==~$ psql -U admin
psql (17.2 (Debian 17.2-1.pgdg120+1))
Type "help" for help.
==admin===# \q
==postgres@9acdf1697be2:~$== psql -U postgres
psql (17.2 (Debian 17.2-1.pgdg120+1))
Type "help" for help.
==postgres===# \c postgres
You are now connected to database "postgres" as user "postgres".
==postgres=#== \c dvdrental  
You are now connected to database "dvdrental" as user "postgres".

##### Borrar la base de datos y recrearla
Como no puedes borrar una base de datos mientras estás conectado a ella, cambia a postgres primero:
- \c postgres
- DROP DATABASE dvdrental;
- CREATE DATABASE dvdrental;
Luego vuelve a conectarte a dvdrental:
- \c dvdrental

##### Ver los usuarios en la base de datos
- SELECT pid, usename, application_name, client_addr, backend_start FROM pg_stat_activity WHERE datname = 'dvdrental';

##### Cerrar todas las sesiones 
- SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE datname = 'dvdrental' AND pid <> pg_backend_pid();
Elimina todas, excepto la sesión en la que estás ejecutando el comando.

##### Restaura los datos manualmente con psql
Si tienes un dump en formato SQL (restore.sql), usa:
- \i /tmp/restore.sql
Si el dump está en formato .tar, usa pg_restore en la terminal y no en psql:
- pg_restore -U admin -d dvdrental /tmp/dvdrental.tar
##### Verifica si los datos están restaurados
- SELECT COUNT(*) FROM pg_tables WHERE schemaname = 'public';
Verifica en una tabla específica
- SELECT * FROM film LIMIT 5;

### Cp .sh a contenedor postgres
1. Primero parar el contenedor
2. Suponiendo que el archivo 1-database-and-user-creation.sh se encuentra en tu máquina local:
	- docker cp /path/to/1-database-and-user-creation.sh <nombre_del_contenedor_postgres>:/docker-entrypoint-initdb.d/
3. Reiniciar contenedor
### Problemas de sesiones usando la base de datos (template1 creado por postgres)
- SELECT pid, usename, application_name, client_addr, backend_start
FROM pg_stat_activity
WHERE datname = 'template1';
##### Matar la sesión
- SELECT pg_terminate_backend(\<pid>);
reemplaza \<pid> con el PID de la sesión que deseas terminar

$$PATH$$
##### Para reemplazar  /tmp en Vim, sin comillas, usa:
- :%s/\$\$PATH\$\$/\/tmp/g
1. %s/ → Aplica el reemplazo en todo el archivo.
2. \$\$PATH\$\$ → Escapamos los signos de dólar ($) con \, porque son caracteres especiales en Vim.
3. /\/tmp/ → Lo reemplazamos por /tmp (las barras / dentro del reemplazo no necesitan ser escapadas).
Confirmar antes de cada cambio
    g → Reemplaza todas las apariciones en cada línea.
    y → Para reemplazar la coincidencia actual.
    n → Para saltarla.
    a → Para reemplazar todas sin preguntar.
    q → Para salir sin hacer más cambios.
#### Reemplaza la ruta en restore.sql
Como los .dat están en /tmp/, abre restore.sql y cambia las líneas que tengan COPY FROM con rutas incorrectas.
    Sustituye \$$PATH$$/3057.dat por /tmp/3057.dat.
    Ejemplo de cambio en restore.sql:
COPY public.actor (actor_id, first_name, last_name, last_update) FROM '/tmp/3057.dat';
Si restore.sql tiene algo como:
- COPY public.actor (actor_id, first_name, last_name, last_update) FROM stdin;
Si también hay líneas con COPY FROM '/tmp/xxxx.dat', hay dos métodos de importación.
Corre la restauración de nuevo
- psql -U postgres -d dvdrental -f /tmp/restore.sql
##### El error indica dos problemas principales:

    "Could not find the file /tmp in container postgres-container":
    Esto significa que el contenedor de Docker no tiene la carpeta /tmp disponible en la ruta especificada o que la ruta no está accesible. En muchos casos, /tmp debería existir por defecto, pero si se ha personalizado la configuración del contenedor, podría ser diferente.

    "Can't add file /home/mooc/Escritorio/postgres/dvdrental.tar to tar: io: read/write on closed pipe":
    Este error está relacionado con un problema en la manipulación de archivos dentro del contenedor. El sistema no puede copiar el archivo al contenedor correctamente debido a un error de lectura/escritura.

Soluciones posibles:

    Verificar la existencia de la carpeta /tmp en el contenedor: Ejecuta el siguiente comando para ingresar al contenedor y verificar si /tmp existe:

- docker exec -it postgres-container /bin/bash
ls /tmp

Si no existe, puedes elegir otra ruta en el contenedor (por ejemplo, /var/lib/postgresql/data o crear una carpeta temporal propia).

Copiar el archivo a otra ruta: En lugar de /tmp, intenta copiar el archivo a una ruta que esté disponible en el contenedor, por ejemplo, el directorio de datos de PostgreSQL:
- docker cp /home/mooc/Escritorio/postgres/dvdrental.tar postgres-container:/var/lib/postgresql/data/dvdrental.tar


# Creación de DB Neon dvdrental
- docker run -d -p 5454:5432 -e POSTGRES_USER=postgres \
-e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=dvdrental \
--name postgres-cont postgres
