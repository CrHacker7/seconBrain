Por convención es mejor que la clase termine con **Listener**.
Tiene los métodos de **...Initialized()** y **...Destroyed()** 
Tiene 3 interfaces: Application, request, y session.
- **ServletContextListener** = Manejar de manera **general**, configuraciones, inicializar recursos globales, base de datos en común para toda la aplicación.
- **ServletRequestListener**= su alcance es una petición. FILTER solo para request.
- **HttpSessionListener** =
1. Se crea la clase con la palabra Listener al final
2. se implementan las 3 interfaces con sus 2 métodos respectivamente.
3. Se añade la anotación @WebListener