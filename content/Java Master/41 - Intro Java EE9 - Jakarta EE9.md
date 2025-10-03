#### Métodos de petición HTTP
get y post a través de una URL
POST es cuando enviamos contenido en el cuerpo del Request.
Si se trabaja con Rest, el contenido se envía en estructura JSON
#### Cabeceras HTTP de petición (request)
Request: enviamos información del cliente, ip,host, navegador, puerto, ruta, si se envía un json
#### Cabeceras HTTP de respuesta (response)
Información que el server envía como respuesta, es el contentType si muestra HTML, imagen, pdf, json, etc
#### Códigos de estado de respuesta HTTP
100 -> INFO
200 -> OK
300 -> REDIRECT
404 -> El recurso no se ha encontrado, o no existe ERROR CLIENT
500 -> Error del server

#### Qué es Java EE?
Es parte de la plataforma java, ejecuta app web usando arquitectura de N capas que se despliegan sobre un servidor de aplicaciones.
- Capa Web MVC = JSP(vista) y JSF -> Model(lógica) -> Servlets(controlador)
- Capa Servicio = EJBs, Servicios Rest
- Capa Datos = Repositorios, Objetos JPA, JDBC
La capa de servicios depende de la capa de datos
La capa de servicio depende de la capa web.

> [!warning] EJB = transaccionales por defecto!

> [!quote] siglas
> **JSP** = Jakarta Server Pages - **JSF** =Jakarta Server Faces **JSP** =Java Single Pages 
> **JCDI** = Jakarta Contexts and Dependency Injection

#### Instalar TOMCAT
1. Descargar la última versión de tomcat.apache.org
2. Descomprimir y copiarlo en una carpeta fuera de descargas
3. abrir tomcat-users.xml con bloque de notas
4. Copiar un user y cambiar el user y pass y roles: 
	`<user username="admin" password="1234" roles="admin,manager-gui,manager-script"/>`
5. Creamos el proyecto y le añandimos las dependencias
```xml
	<packaging>war</packaging>
	<dependencies>
        <dependency>
            <groupId>jakarta.platform</groupId>
            <artifactId>jakarta.jakartaee-api</artifactId>
            <version>9.0.0</version>
            <scope>provided</scope> 
        </dependency>
    </dependencies>
    
```
Este `scope` de jakarta incluye todas las clases de Jakarta EE 9 **api-servlet**, el **JSP** y las **libreries**. ==IMPORTANT!==
6. Añadir 2 plugins: Maven-compiler y el Tomcat7
```xml
	<build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId> <!-- como es oficial se puede omitir esta linea-->
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.13.0</version>
            </plugin>
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
                <configuration>
                    <url>http://localhost:8080/manager/text</url>
                    <username>admin</username>
                    <password>1234</password>
                </configuration>
            </plugin>
        </plugins>
    </build>
```
7. Creamos una hrna de java un new Directory; en main click derecho -> new Directory
8. Nombramos al nuevo dir **webapp** es un nombre por convención, y dentro un archivo html llamado index (no es necesario poner .html)
9. edit configuration -> add next... -> maven, cambiamos el nombre a tomcat7, Command Line : tomcat7:redeploy -> apply
10. Levantar el servidor en la terminal e ir al path: 
	1. mooc@mooc-VirtualBox:~/Descargas/apache-tomcat-10.1.43/bin$ ==./==startup.sh

> [!SUMMARY] Terminal ejemplo
```js
mooc@mooc-VirtualBox:~/Descargas/apache-tomcat-10.1.43/bin$ ./startup.sh
Using CATALINA_BASE:   /home/mooc/Descargas/apache-tomcat-10.1.43
Using CATALINA_HOME:   /home/mooc/Descargas/apache-tomcat-10.1.43
Using CATALINA_TMPDIR: /home/mooc/Descargas/apache-tomcat-10.1.43/temp
Using JRE_HOME:        /usr
Using CLASSPATH:       /home/mooc/Descargas/apache-tomcat-10.1.43/bin/bootstrap.jar:/home/mooc/Descargas/apache-tomcat-10.1.43/bin/tomcat-juli.jar
Using CATALINA_OPTS:   
Tomcat started.
```

> [!NOTE] Menor a JDK-16
> <!--            menor a la versión jdk-16 usar el plugin de abajo-->
<!--            <plugin>-->
<!--                <artifactId>maven-war-plugin</artifactId>-->
<!--                <version>3.2.3</version>-->
<!--                <configuration>-->
<!--                    <failOnMissingWebXml>false</failOnMissingWebXml>-->
```JS
          <plugin>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.2.3</version>
        //SI ERROR MSG 'WEB-INF' PONER ESTA LINEA PORQUE NO USAREMOS WEB-INF, LO HAREMOS CON API-SERVLET USANDO ANNOTACIONES.'SI FALLA SE LO SALTA CON FALSE' 
                <configuration>
                    <failOnMissingWebXml>false</failOnMissingWebXml> 
                </configuration>         
          </plugin>-->
```

11. Entramos al path que le dimos en el pom o a manager
	1. http://localhost:8080/webapp http://localhost:8080/manager
12. En 'archivos WAR a desplegar' vamos a nuestro proyecto -> target -> webapp-1.0-SNAPSHOT.war lo seleccionamos y desplegamos. Con esto tendremos 2 webapp, la por defecto y la que hemos desplegado de forma manual.
13. Para detener el despliegue en la consola, `CTRL + C` o `./shutdown.sh` finaliza el servidor.
14. Para volver a levantar TOMCAT -> `~/Descargas/apache-tomcat-10.1.43/bin$ ./startup.sh`
15. Por cada cambio que hagamos, desplegar RUN, que se genere el WAR otra vez.

> [!failure] Error de puerto
> Es posible que ya esté levantado y nos aparezca un error que no se puede levantar.

#### Qué es un Servlet?
Es una clase y objeto JAVA utilizado para implementar una página web dinámica con carácteristicas HTTP de petición y respuesta.

- HttpServlet (Clase principal)= heredamos de esta.
- GenericServlet (Clase abst)= 
	service() es un método abst, no tiene implementación
- Servlet (Interfaz) = 
	contiene los métodos init(), service() y destroy() = LIFECYCLE
- HttpServletResponse = implementa la interfaz ServletResponse
- HttpServletRequest = implementa la interfaz ServletRequest.

> [!success] Lifecycle de un Servlet
> Se compila y registra en el contenedor
> Init() para inicializar, solo ocurre una vez, como un constructor.
> Por cada Request y Response se crea un único hilo en el ciclo de vida y el service() lo deriva al doGet() o doPost(), al que corresponda
> destroy() para cerrar todo, si abrimos la tenemos que cerrar
> Importante que un **Servlet** no maneje estados **stateless** porque si modificamos un atributo, se reflejará en todos los usuarios, si queremos hacerlo, es mediante HttpSession()

> [!warning] Por convención en la clase se pone la palabra **servlet** ej: HolaMundoServlet
> - @WebServlet("/hola-mundo) = asociamos una ruta a esta clase Servlet, no caracteres especiales, es una URL, así que se le pone encima de toda la clase.
> - NUNCA se sobreescribe el método service() solo los doGet(), doPost(), etc

> [!bug] Error 404 al implementar @WebServlet
> Tiene que coincidir el Java del ordenador con el Java del proyecto.

> [!todo] Live Template en Intellij IDEA 
> Crear un template en el IDEA -> ctrl + alt + s -> live template -> new Live template -> le damos un nombre y pegamos el texto quitando los espacios de la izquierda; las tabulaciones. 
> Cambiamos los títulos envuelto con el signo de dolar para que sea modificable. `$title$` . Edit variables -> y aplicamos un valor por defecto: Hello World! -> ok. No applicable context- Define -> java ->apply y ok.
> Para aplicarlo ponemos el nombre que le hemos dado al template.

- getParameter("saludo"); ese nombre de string es el que se usa para llamarlo en la url.
- El primer parámetro siempre va con el signo de pregunta nombre=valor, el resto es con ampersand`&`
- Al enviar en el cuerpo del mensaje con post() es más seguro y significa que no aparecen los datos en la URL.

> [!NOTE] Devuelven:
> value**O**f -> Entero de referencia (**O**bject)
> **p**arseInt -> Entero **p**rimitivo 

> [!WARNING] SUMMARY
> 1. pom (packaging, jakarta.jakartaee-api, compiler, tomcat7 = configuration: url,user,pass)
> 2. en main ->new dir -> webapp
> 3.  edit configuration -> add next... -> maven, cambiamos el nombre a 'tomcat7', Command Line : tomcat7:redeploy -> apply
> 4. clase que contenga Servlet al final y @WebServlet arriba de cada clase
> 5. ALT + INSERT = @Override doPost() o doGet(), la class extends HttpServlet y  con throws ServletException, IOException
> 6. setContentType("/text/html);
> 7. PrintWriter
> 8. Plantilla html

> [!tip] Plantilla de Pom.xml
> ctrl + alt + s -> live template -> seleccionar maven -> **+** -> Live template -> abbreviation=javaeemvn -> pegamos la dependencia y plugins -> Define=maven -> apply

