1. Descargar el wildfly 
2. añadir dep
```xml
	<dependency>
		<groupId>jakarta.platform</groupId>
		<artifactId>jakarta.jakartaee-api</artifactId>
		<version>9.1.0</version>
		<scope>provided</scope>
	</dependency>
<plugins>
	<plugin>
		<artifactId>maven-compiler-plugin</artifactId>
		<version>3.8.1</version>
	</plugin>
	<plugin>
		<groupId>org.wildfly.plugins</groupId>
		<artifactId>wildfly-maven-plugin</artifactId>
		<version>2.1.0.Final</version>
	</plugin>
	<plugin>
		<artifactId>maven-war-plugin</artifactId>
		<version>3.2.3</version>
		<configuration>
			<failOnMissingWebXml>false</failOnMissingWebXml>
		</configuration>
	</plugin>
</plugins>
```
3. Para arrancar desde terminal : ~/Descargas/wildfly-preview-25.0.0.Final/bin$ **./standalone.sh** 
4. configurar el run : **wildfly:deploy**
5. verificar si ok en terminal: WFLYSRV0010: Deployed "webapp-ejb.war" (runtime-name : "webapp-ejb.war")
6. nombre proyecto y ruta para verlo en el navegador sin .war:  WFLYEJB0473: JNDI bindings for session bean named 'ServiceEjb' in deployment unit 'deployment "**webapp-ejb**.war"' are as follows: y la ruta del servlet

Es una EJB sin estado, o sea, es compartido por todos los clientes. Si un cliente lo modifica, todos se ven afectados.


