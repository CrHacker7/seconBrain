> [!tip] atajos de catch
>1. **try(Connection conn = ConnectionDatabase.getConnection()){}** 
> 	sobre el error ALT + ENTER = add catch clauses.
>2. **public static Connection getConnection() {return DriverManager.getConnection(url,username,password);}**
> 	Add exception to method signature.

> [!bug] className solution
> Pero en ciertos entornos (como Servlets, filtros, o cuando el classloader es especial) no siempre se carga automáticamente.
> Por eso, llamar explícitamente a Class.forName() evita que DriverManager no encuentre ningún driver para tu URL.
> -Verificar que **target/web-inf/lib/(*.jar)** especialmente mysql-connector

> [!error]
> **Estado HTTP 500 – Internal Server Error**
> Tipo Informe de Excepción
> mensaje java.sql.SQLException: **No suitable driver found for** jdbc:mysql://localhost:3306/java_course?serverTimezone=UTC
> Descripción El servidor encontró un error interno que hizo que no pudiera rellenar este requerimiento.
> 	java.sql/java.sql.DriverManager.getConnection(DriverManager.java:702)
> 	java.sql/java.sql.DriverManager.getConnection(DriverManager.java:228) 	org.cr7.apiservlet.webapp.cookie.filters.ConnectionFilter.doFilter(ConnectionFilter.java:17)


