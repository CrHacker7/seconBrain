- Añadir a nuestras plantillas javaeemvn 
	- `<project.reporting.outEncoding>UTF-8</project.reporting.outEncoding>`
		Mover esta línea properties.
	- Añadir `<finalName>${project.artifactId}</finalName>`
	- Añadir `<packaging>war</packaging>` dentro build block
- Añadir charset en la plantilla user: resp.setContentType("text/html**;charset=UTF-8**");

#### Creamos nuestro proyecto:
1. Crear clase con Servlet al final.
2. Extender de **HttpServlet**
3. Añadir anotación **@WebServlet**("/cabeceras-request")
4. Implemetar method **doGet();**
5. Quitar el super del method e implementar **outhtml**

> [!warning] Variables de cabeceras
> - String metodoHttp = **req.getMethod();** 
> 	- *Devuelve el metodo doGet() o el que tengamos* 
> 		- **GET**
> - String requestUri = **req.getRequestURI();** 
> 	- *Devuelve nombre proj. y el mapping de la url del servlet*
> 		- **/webapp-headers/cabeceras-request**
> - String requestUrl = **req.getRequestURL().toString();** 
> 	- *Devuelve un StringBuffer y por eso toString() para devolver la cadena completa*
> 		- **http://localhost:8080/webapp-headers/cabeceras-request**
> - String contextPath = **req.getContextPath();** 
> 	- *Devuelve el nombre del proyecto, o contexto*
> 		- **/webapp-headers**
> - String servletPath = **req.getServletPath();** 
> 	- *Devuelve la ruta del Servlet, obtener de forma dinámica la cabecera request*
> 		- **/cabeceras-request**
> - String ip = **req.getLocalAddr();**
> - int port = **req.getLocalPort();**
> - String scheme = **req.getScheme();**
> - String host = **req.getHeader("host");**

Al levantar la web nos dará 404, así que añadir la uri que le dimos en el WebServlet

> [!warning] Title
> HTTP = GET
> CONTEXT = /webapp-request
> SERVLET = /cabeceras-request
> URI =  context + servlet
> URL = scheme + server name + port + uri

> [!success] Building URL
> String url = **scheme** + "**://**" + **host** + **contextPath** + **servletPath;**
> String url2 = scheme + "://" + ip +  ":" + port + contextPath + servletPath;

**localAddr()** Returns the IP addr of the interface on which the request was received
**getRemoteAddr()** Returns the IP addr of the client or last proxy that sent the request. For HTTP servlets, same as the value of the CGI variable REMOTE_ADDR

- req.getHeaderNames(); 
	- CTRL + ALT + V = Enumeration\<String> headersNames (Crea la variable )

> [!TODO] Export to Excel
> @WebServlet(**{"/productos.xls", "/productos.html"}**)
> String servletPath = req.getServletPath();
> boolean isXls = servletPath.endsWith(".xls");
>    if (isXls) {
>     **resp.setContentType("application/vnd.ms-excel");**
>     **resp.setHeader("Content-Disposition", "attachment;filename=productos.xls");**
>    }
>    Añadimos un if después del try y dentro ponemos la tabla
>    le preguntamos si contiene .xls al servletPath y  lo convertimos en boolean. Al final de la tabla añadimos otro if 

**String servletPath = req.getServletPath();**
**¿Qué hace?**
Obtiene el path con el que se llamó al servlet, por ejemplo /productos o /productos.xls.

**boolean isXls = servletPath.endsWith(".xls");**
**¿Qué hace?**
Pregunta si la URL termina en .xls → eso indica que el usuario quiere descargar la lista como archivo Excel.

**¿Por qué así?**
Para saber si debe generar HTML o contenido Excel con la misma lógica de negocio (la lista de productos es la misma).

### API-REST
#### HEADERS RESPONSE GENERAR RESPUESTA EN JSON 
 ```XML
 1. En el pom.xml
<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-databind</artifactId>
	<version>2.17.1</version>
</dependency>
 ```
2. ObjectMapper mapper = new ObjectMapper();
3. String json = mapper.writeValueAsString(productos);
4. resp.setContentType("application/json");
5. resp.getWriter().write(json);
6. Añadir link al index, tiene que ser igual que el @WebServlet 
#### HEADERS (ENVIAR UN OBJ. JSON EN EL CUERPO DEL REQUEST)
Aquí sería como recibir un formulario del lado del cliente, pero esta vez recibiremos un objeto json dentro del Request.
1. Hacer un **doPost()**
2. **req.getInputStream()** (ctrl+alt+v=convertir en variable) *es una corriente en byte*
3. Convertir este stream en un Objeto Producto, no la lista completa.
	1. Instanciamos la class **ObjectMapper()**
	2. Convertimos un obj de tipo ServletInputStream a tipo Producto
		1. Producto prod = **mapper.readValue(jsonStream, Producto.class)**

#### HEADERS (RESPONSE LOCATION VS DISPATCHER FORWARD)
 1. **resp.setHeader("Location", req.getContextPath() + "/productos.html");**
 2. **resp.setStatus(HttpServletResponse.SC_FOUND);** // COD-302

> [!SUMMARY] REDIRECT
> **resp.sendRedirect(req.getContextPath() + "/productos.html");**
> Esta línea hace las dos de arriba en una sola.
> - Se usa para realizar una nueva petición.
> - Se recarga la página y se pierden los atributos del request.


El RequestDispatcher preferible en muchos casos porque permite reenviar la petición a una JSP sin hacer un redireccionamiento completo. Esto significa que no se pierde el estado de la petición ni los datos (como los atributos del request) al cambiar de servlet a JSP.
Al usar un dispatcher, el servlet y la JSP trabajan juntos como parte del mismo ciclo de petición (es decir, un "forward" interno). No hay recarga de página en el navegador ni cambio visible en la URL.
Cuando un servlet necesita mostrar una vista, realiza un forward con RequestDispatcher, lo cual permite "unir" la lógica del controlador (servlet) con la parte visual (JSP). Los parámetros o atributos que se establecen en el request desde el servlet son accesibles directamente desde la JSP. Pasamos los params del controlador a la vista realizamos un dispatcher en el request.

#### STATUS HTTP RESPONSE 401 NOT AUTHORIZED

> [!todo] login.html
> **name** enlazado al nombre del getParameter() del request
> **for** enlazado al id

mensaje de error en el else
`resp.sendError(HttpServletResponse.SC_UNAUTHORIZED, "You are not authorized to access this website!" );`

> [!success] Redirect or not?
> Podemos verificar si es dispatcher o location yendo a la pestaña Network de DevTools

#### STATUS HTTP RESPONSE 404 NOT FOUND
**found.get().getName()** -> con get() se obtiene el objeto y luego su nombre.



> [!summary] BOXES
> **Optional**: una cajita que puede tener o no algo dentro.
> **Future**: una caja que se llenará más tarde.
> **Stream**: una cinta transportadora con muchos objetos.
> **Result**/**Either**: caja con un éxito o un error adentro.
> **List**: una caja con muchos objetos.

==SI NO TENEMOS EL **METHOD** EN EL **FORM**, BUSCA POR DEFECTO EL **doGet()** Y DARÁ FALLO PORQUE TENEMOS UN **doPost()** EN EL SERVLET==

- Si no ponemos nada en buscar, encontrará el primero alfabéticamente porque cualquier  String empiezan con un caracter vacío y por eso coincide.
- **contains()** no necesitamos escribir todo el título pero sí es sensitiveCase. Tendríamos que validarlo para que sea más estricto.



