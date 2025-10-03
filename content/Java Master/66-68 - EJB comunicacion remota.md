**Serializable** se aplica cuando el objeto se va a transportar de forma remota desde un servidor hacia un cliente, pero si está dentro de la misma aplicación no es necesario, por convención no es necesario, aunque podría variar.
- en el modelo tenemos que implements Serializable y añadir su atributo        `static final long serialVersionUID = 42L;`

Para crear la app .jar
1. Borrar del pom el packaging y el plugin relacionado al war.
2. Creamos otro proyecto y copiamos el groupId, artifactId, version del primer proyecto y lo pegamos en el segundo como una dependencia, dará fallo, así que en el primer proyecto hacemos ***maven install***. Así instalamos en el repo local de maven para que encuentre la librería-dependencia, es decir, se publica en el repo local de maven.
3. ir a la web de wildfly, https-remoting: https://docs.wildfly.org/25/Developer_Guide.html#JNDI_Remote_Reference copiamos la dep en el pom del lado del **cliente**
```xml
<dependency>
  <groupId>org.wildfly</groupId>
  <artifactId>wildfly-ejb-client-bom</artifactId>
  <version>11.0.0.Final</version>
  <type>pom</type>
</dependency>
```
4. en la clase main del cliente, añadimos el código que aparece en la misma página donde se muestra la dependencia anterior."
```java
final Properties env = new Properties();
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.jboss.naming.remote.client.InitialContextFactory");
env.put(Context.PROVIDER_URL, "http-remoting://localhost:8080");
// the property below is required ONLY if there is no ejb client configuration loaded (such as a
// jboss-ejb-client.properties in the class path) and the context will be used to lookup EJBs
env.put("jboss.naming.client.ejb.context", true);
InitialContext remoteContext = new InitialContext(env);
//anidar el InitialContext con try/catch
```
   5. en el lookup añadimos --> *ejb:/nombre-proyecto/NombreClase!package.InterfazRemota*
```java
service = (ServiceEjbRemote) remoteContext.lookup("ejb:/appejb-remote/ServiceEjb!org.cr7.webapp.ejb.services.ServiceEjbRemote")
```
6. después de cada modificación se tiene que actualizar el **maven install**, redeploy.
7. En el cliente, creamos un archivo dentro de la carpeta resources, y luego hacemos (Ctrl + clic) sobre las constantes INITIAL_CONTEXT_FACTORY y PROVIDER_URL para copiar sus valores. Esos valores los pasamos como variables en el archivo, tomando el contenido que aparece entre comillas en el método main. 
```python
jndi.properties (nombre del archivo)
java.naming.factory.initial=org.jboss.naming.remote.client.InitialContextFactory
java.naming.provider.url=http-remoting://localhost:8080
jboss.naming.client.ejb.context=true
```

#### Despliegues en consola admin web de Wildfly
1. en la consola `~/Descargas/wildfly-preview-25.0.0.Final/bin$` ***./add-user.sh***
2. tipo de user, damos Enter, por default es admin; añadir user y pass, yes; en el group lo dejamos en blanco, enter; pregunta si es correcto el nombre de user, yes; pregunta si queremos conectar este user a otro proceso "slave host controller connecting to the master..." no. Levantamos ***./standalone.sh***

> [!warning] puerto 8080 ocupado
> Para cambiar el puerto ir a: **wildfly/standalone/configuration/standalone.xml** y cambiamos la línea con el puerto 8181: **`<socket-binding name="http" port="${jboss.http.port:8080}"/>`** 
> - NO olvidar de verificar el puerto también en el file jndi.properties.
 
3. entramos en *administration console* con el user y pass que añadimos, Deployments -> start. Nos aparecerá las dos aplicaciones (.jar .war), damos clic al .jar y undeploy. En la consola tendremos la confirmación: *WFLYSRV0009: Undeployed "appejb-remote.jar" (runtime-name: "appejb-remote.jar")*
#### Para añadir deploy manualmente
1. hacemos maven clean, install, en el proyecto remoto.
2. en la interfaz de wildfly -> Deployments -> upload deployment -> seleccionamos el .jar de la carpeta target, next, finish. Y la confirmación en la consola: *WFLYSRV0010: Deployed "appejb-remote.jar" (runtime-name : "appejb-remote.jar")*
# Sección 67 - Desploy y estructura EAR
#### Creando estructura modular

1. creamos un proyecto vacío (sin maven) name: **webapp-ear-module**
2. file -> new -> module... -> maven -> name: **webapp-ear-ejb**
3. file -> new -> module... -> maven -> name: **webapp-ear-war**
4. file -> new -> module... -> maven -> name: **webapp-ear-ear**
5. Añadir esta base a todos los pom.xml
```xml
	<packaging>ejb</packaging> --puede ser war, jar, ejb
    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    </properties>
    <dependencies>
        <dependency>
            <groupId>jakarta.platform</groupId>
            <artifactId>jakarta.jakartaee-api</artifactId>
            <version>9.1.0</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    <build>
	    <finalName>${project.artifactId}</finalName>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
            </plugin>
        </plugins>
    </build>
```


> [!tip] webapp-ear-war
```xml 
<groupId>org.cr7.webapp.ear</groupId>
    <artifactId>webapp-ear-war</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>
    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.cr7.webapp.ear</groupId>
            <artifactId>webapp-ear-ejb</artifactId>
            <version>1.0-SNAPSHOT</version>
            <type>ejb</type>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>jakarta.platform</groupId>
            <artifactId>jakarta.jakartaee-api</artifactId>
            <version>9.1.0</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    <build>
        <finalName>${project.artifactId}</finalName>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
            </plugin>
            <plugin>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.4.0</version>
                <configuration>
                    <failOnMissingWebXml>false</failOnMissingWebXml>
                </configuration>
            </plugin>
        </plugins>
    </build>
```


> [!tip] webapp-ear-ejb
```xml
<groupId>org.cr7.webapp.ear</groupId>
    <artifactId>webapp-ear-ejb</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>ejb</packaging>
    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    </properties>
    <dependencies>
        <dependency>
            <groupId>jakarta.platform</groupId>
            <artifactId>jakarta.jakartaee-api</artifactId>
            <version>9.1.0</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    <build>
        <finalName>${project.artifactId}</finalName>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
            </plugin>
            <plugin>
                <artifactId>maven-ejb-plugin</artifactId>
                <version>3.2.1</version>
                <configuration>
                    <ejbVersion>3.2</ejbVersion>
                </configuration>
            </plugin>
        </plugins>
    </build>
```


> [!tip] webapp-ear-ear (add ejb & war as a dep)
```xml
<groupId>org.cr7.webapp.ear</groupId>
    <artifactId>webapp-ear-ear</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>ear</packaging>
    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    </properties>
    <dependencies>
        <dependency>
            <groupId>jakarta.platform</groupId>
            <artifactId>jakarta.jakartaee-api</artifactId>
            <version>9.1.0</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.cr7.webapp.ear</groupId>
            <artifactId>webapp-ear-ejb</artifactId>
            <version>1.0-SNAPSHOT</version>
            <type>ejb</type>
        </dependency>
        <dependency>
            <groupId>org.cr7.webapp.ear</groupId>
            <artifactId>webapp-ear-war</artifactId>
            <version>1.0-SNAPSHOT</version>
            <type>war</type>
        </dependency>
    </dependencies>
    <build>
        <finalName>${project.artifactId}</finalName>
        <plugins>
            <plugin>
                <groupId>org.wildfly.plugins</groupId>
                <artifactId>wildfly-maven-plugin</artifactId>
                <version>2.1.0.Final</version>
            </plugin>
            <plugin>
                <artifactId>maven-ear-plugin</artifactId>
                <version>3.2.0</version>
                <configuration>
                    <modules>
                        <webModule>
                            <groupId>org.cr7.webapp.ear</groupId>
                            <artifactId>webapp-ear-war</artifactId>
                            <contextRoot>/webapp-ear</contextRoot>
                            <bundleFileName>webapp-ear-war.war</bundleFileName>
                        </webModule>
                        <ejbModule>
                            <groupId>org.cr7.webapp.ear</groupId>
                            <artifactId>webapp-ear-ejb</artifactId>
                            <bundleFileName>webapp-ear-ejb.jar</bundleFileName>
                        </ejbModule>
                    </modules>
                </configuration>
            </plugin>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
            </plugin>
        </plugins>
    </build>
```
> [!todo] plugins: widlfly y plugin configuration ejb y war
> - Añadimos el plugin wildfly.
> - Dentro del ear añadimos un plugin con la configuration del groupId y artifactId del ejb y war.
> - Y la dep de mysql (solo se descarga en el target, luego la eliminamos del pom)
> - verificamos que está descargada, en el target: ...mysql-connector..
> - esto se hace para subirla al server, luego también la eliminamos del target

> [!Error] El WAR depende del EJB 
> Primero hacemos install del **ejb** luego del **war** para instalar de forma local estas dependencias.
> y después del **ear**(maven package) porque no es una dependencia.

6.  levantamos el wildfly, nos logeamos y deployments, start.
7. seleccionamos el target, carpeta webapp-ear-ear, escogemos el jar del mysql
8. Hacemos clean para quitar todos los targets de ejb, war y ear.
9. En el management console -> configuration -> subsystems -> Datasources & Drivers -> Datasources -> Add Datasource - >MySql -> (copiar el jndi) -> Driver Name: mysql-connector...jar ->Connection (jdbc:mysql://localhost:3306/java_course?serverTimezone=Europe/Zurich) user y pass de la bbdd

> [!error] becareful mysql-connector 8.0.33, wrong version!
```xml
<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.27</version> // v8.0.33 is wrong!
        </dependency>
```

#### Implementando repositorio con JPA (componente CDI) y service EJB en module ejb
1. dirigirse a la pág de jakarta.ee -> specification -> browse spec -> Jakarta Contexts and Dependency Injection -> Jakarta Contexts and Dependency Injection 3.0(jakarta ee9) -> Jakarta Contexts Dependency Injection 3.0 Specification Document (HTML) -> (ctrl+f= beans.xml) copiamos el xml que nos aparece en el apartado 5.1.1.2. Declaring selected alternatives for a bean archive.
2. Y **webapp-ear-war**: en main crear un dir *webapp*, dentro otro dir *WEB-INF* y dentro un file *beans.xml*. copiamos el beans.xml, quitando todo el alternatives.

> [!tip] important!
> - @Inject sí toma en cuenta el contexto CDI (servlet), en cambio @EJB no lo hace
> - Los ejb manejan de forma automática las transacciones (quitar @Transactional)
> - Los @Stateless no manejan scope (quitar @ApplicationScope)

Ejb es todo lo que maneja transacciones en sus métodos, como los services

##### Migrar de tomcat (jpa) a wildfly (ejb)
1. Quitar todos los transactional, ejb ya los maneja en automático y poner @Stateless en todas las clases impl
2. Cambiar el persistence.xml, transaction-type, quitar las properties que indican el user, pass y conexión (wildfly lo manejará)
3. En ProducerResources añadir @PersistenceUnit con nombre que hay en el file persistence.xml en el tag persistence-unit, inyectar el entityManagerFactory que al llamarlo nos creará un emf para usar JPA
```java 
    private EntityManager beanEntityManager(){
        return emf.createEntityManager();
```
     `cambiar @Resource(name = "java:/MySqlDS")` POR 
     `@Resource(lookup = "java:/MySqlDS")`
2. Quitar los interceptores de transactional, salvo logging
3. En el beans.xml quitar las clases de transactional
4. Añadir anotación @Local a service 
5. Quitar la conexión a la bbdd del web.xml del tag resource-ref
6. Quitar el tag resource del context.xml, dejando un tag Context vacío
- @Stateless no puede manejar contexto.
- Los services tienen que ser transaccionales, tienen que ser ejb, sino no funciona las operaciones CRUD.

##### Otra manera de hacer la migración
1. en ProducerResources duplicar y renombrar a ProducerEntityManager y en el original quitar 
2. @Stateful se asocia con @RequestScope
3. EntityManager manejado por el contenedor JPA y contenedor wildfly, el repo del @RequestScope, el service del @RequestScope y @Stateful, al ser @Stateful es ejb maneja transacciones.
4. Se cambia @PersistenceUnit por @PersistenceContext
