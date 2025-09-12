El getParameter siempre devuelve un String y lo convertimos a Long.
Como findById(id) espera un Long, necesitas convertir el string a Long, por eso haces:
**Long id = *Long.parseLong*(req.getParameter("id"));**

> [!Error] Casting
>- La comparación de String contra String no lanza excepciones, solo falla la comparación si no coinciden.  
>- Convertir a Long o algún literal numérico y hacer la comparación, ya que explotaría si no se recibe el formato numérico y nos daría el NumberFormatException.
>  

- **equals(Long.toString(...))**
	Convierte Long → String	String contra String	Seguro si el ID no es null
- **Long.parseLong(...) == Long**	
	Convierte String → Long	numérico contra numérico	Puede lanzar NumberFormatException si el string no es válido


> [!bug] Error 401
> Eliminar la carpeta target y volver a levantar la app.


