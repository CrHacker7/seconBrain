- HttpSession es otra opción para al almacenar datos del usuario que sean persistentes en diferentes request y se almacena en el lado del servidor.
- Las cookies se guardan en el lado del cliente, en el navegador.
- Una session nos permite guardar objetos completos con todos sus estados, valores de sus atributos.

> [!summary] Session Methods
> - Crear una sesión http
> 	HttpSession session = **request.getSession();**
> - Obtener un Objeto
> 	String username = **session.getAttribute("username");**
> - Guardar un Objeto en la sesión http
> 	**session.setAttribute("username", usuario);**
> - Eliminar un valor de la sesión del cliente
> 	**session.removeAttribute("username");**
> - Eliminar o invalidar la sesión actual del cliente
> 	**session.invalidate();**
> - **isNew()**
> - **getCreationTime()**
> - **getLastAccessedTime()**
> - **getMaxInactiveInterval()**
> - **setMaxInactiveInterval(int interval)**

==SHIFT + F6 = Sobre una variable renombra a todas.==
