> [!note] Plantilla de Pom.xml
> ctrl + alt + s -> live template -> seleccionar maven -> **+** -> Live template -> abbreviation=javaeemvn -> pegamos la dependencia y plugins -> Define=maven -> apply

- Los formularios tienen el atributo **action**="/webapp-form/registro" especifica el target que recibirá el formulario, aquí va la ruta url que está mapeada el servlet, ("/nombre_del_proyecto/nombre _de _la_pag") después, el método de enviar tiene que ser POST para que se envíe en el cuerpo del mensaje del request, así no será visible en la URL como doGet()
- Por defecto, sin method en el form usa GET, por ende se verá en la URL.
- Matchear el atributo method del form con el método de la clase doPost() o doGet()

> [!important]- La ruta del **form action** tiene que matchear con el **@WebServlet** de la clase

- es mejor usar post para que no se vean datos sensibles, además que se pueden enviar más carácteres en el request, mediante URL tiene un límite.
- El atributo for del **label** tiene que matchear con los atributos del **input**(name, **id**)

1. edit configuration -> maven -> command line= tomcat7:redeploy -> apply
2. levantar el tomcat desde /bin -> ==./startup.sh==

> [!bug]
> - Si error al deployar, revisar el command line a **redeploy** 
> - Error: *Estado HTTP 405 – Method Not Allowed* ->eliminar carpeta **target**

- Al crear una lista select desplegable, y poder escoger varias tenemos que poner **multiple** en el tag: **\<select name="languages" id="languages" *multiple*>**
- Para seleccionar por defecto desde el inicio:
	**\<option value="jakartaee" *selected*>Jakarta EE\</option>
	\<input type="checkbox" name="roles" value="ROL_ADMIN" *checked*>**

- String country = req.getParameter("**country**"); se captura el **name** de los atributos de los tags del html
```js
Arrays.asList(roles).forEach(role ->{
                out.println("        <li>" + role + "</li>");
        //el forEach sirve para hacer click en el label, el cursor se posiciona en el campo 
```


> [!hint] Cambiar el nombre de proyecto en el **target**
>  `<build><finalName>webapp-form</finalName>` Así el archivo será webapp-form.war (sin -SNAPSHOT).
>  También podemos usar `<finalName>{project.artifactId}$</finalName>`
>  1.   main menu -> Build -> build project
>  2.   Run tomcat

- El **id** de los atributos del html son para que se queden marcados por el cursor al darle click en el **label** mediante el **forEach**
- Para crear un campo **hidden** se envía por debajo del html, no se ve, técnicamente va antes de cerrar el formulario.
	- Lo capturamos en el servlet: con el req.getParameter()
#### VALIDACIÓN DE FORMULARIO
equals("") -- equals(" ") = isBlank() -> valida vacío y espacio en blanco (java11)
#### MENSAJES DE ERROR EN VISTA JSP
1. Pasamos los msg de error a la vista con el **req.setAttribute()**;
2. Para cargar la lista al JSP **getServletContext()**, maneja el contexto de los servlets y el método **getRequestDispatcher()** y luego el método **forward()** para redireccionar o cargar el JSP y le pasamos **req y resp**
3. Ponemos la validación dentro de un **if**; out.println(); y en el **else** no tiene nada de html.
4. Renombrar la extensión del index.html a index.jsp
5. Se le pone directivas, arriba del todo del index.jsp por intellij no tiene soporte: 
```java
	<%@page contentType="text/html" pageEncoding="UTF-8"%>
	<%@page import="java.util.List"%>
	<% esto es un SCRIPTLET
	  List<String> errors = (List<String>)request.getAttribute("errors");
	%>
	//DEBAJO DEL FORM PONEMOS LOS SCRIPTLET
	
	<%
	    if(errors != null && errors.size() > 0) {
	%>
	<ul>
	<% for(String error : errors) { %>
	    <li><%= error %></li>
	    <li><% out.print(error); %></li> //otra forma de imprimir
	<% } %>
	</ul>
	<% } %> 
```
```js
req.setAttribute("errors", errors);
            getServletContext().getRequestDispatcher("/index.jsp").forward(req, resp);
```
> [!SUMMARY] RESUMEN
> Le pasamos los mensajes de error al **contexto** del request con **setAttribute()** y cargamos la vista JSP

> [!note]
> **getParameters()** son los que envía el usuario
> **setAttribute()** son los que permiten pasar datos de un Servlet a otro o a un JSP, estos attributes son compartidos por los servlets y JSP dentro del request

<%=error%> -> esto es una expresión porque imprime una variable en HTML DE JSP


> [!BUG] Errors
La variable req no está definida. En JSP, el objeto correcto es request, que es una variable implícita (disponible por defecto en el contexto).

> [!note]
> Para poder mostrar los msjs de errores es necesario convertir el html en vista JSP para que soporte código java, los msjs de error se los pasa el Servlet a la vista 

Para validar al lado del campo
1. Crear un mapa para enlazar el nombre por el mensaje de error.
2. Usar el método put(k, v) y setAttribute("mapErrors", mapErrors); en el else del servlet.
3. En JSP añadimos el import y el hashmap y en el for hacemos que nos devuelva con el forEach el mensaje con **mapErrors.values()**
4. validar que no sea null y sea mayor a 0 e iteramos todos los mensajes y pintamos
5. Debajo de cada campo añadir un **if** validando que no sea null y que **containsKey**("username") y pintamos **mapErrors.get**("username")
```java
<%
 if(mapErrors != null && mapErrors.containsKey("username")){
	 out.println("<small style='color: red;'>" + mapErrors.get("username") + "</small>");
 }
%>
```

> [!success] Clean
> Icono maven -> Lifecycle -> clean = se borra la carpeta **target** y volvemos a RUN

#### BOOTSTRAP
**Agregar estilos de forma local en el proyecto**
1. ver mi código fuente y  click en el link de bootstrap
2. Click derecho, guardar como...
3. Copiarlo en la carpeta de nuestro proyecto en el dir **webapp** crear otro dir **style** y  lo pegamos dentro
4. Quitar la primera parte del link hasta /style... y quitamos todo lo demás, queda así:
```
<link href="/style/bootstrap.min.css" rel="stylesheet">
```
5. Añadimos por delante con una expresión el proyecto renombrado:  
`<link href="<%=request.getContextPath()%>/style/bootstrap.min.css" rel="stylesheet">`
De esta manera saldrá en el código fuente el nombre y no el link de bootstrap.

##### MANTENER VALORES DEL FORMULARIO Y SELECCIONES AL VALIDAR

> [!summary] `${param.<var>}`
> - TEXT *input* (user, pass, email) -> `value="${param.username}`
> - SELECT *option/input* (lista countries, languages, roles) -> 
> 	`${param.country.equals("ES")? "selected": ""}`
> 	`${param.country.equals("ES")? "checked": ""}`
> - MULTI SELECT *option* (progLanguages, roles)  `${paramValues.progLanguages.stream().anyMatch(v -> v.equals("sql")).get()?"selected":""}}`
> - `${paramValues.roles.stream().anyMatch(v -> v.equals("ROLE_ADMIN")).get()?"checked":""}} `
>-  RADIO *input* `${param.language.equals("es")? "checked": ""}`

