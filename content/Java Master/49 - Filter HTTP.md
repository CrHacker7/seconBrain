**Diferencia con los listener?**
- Los **filter http** son 100% orientado al request, a manejar el ciclo de vida, cuando inicia un request y cuando finaliza.
- Podemos seleccionar en qué servlet se va a ejecutar, podemos mapear a una ruta URL que está mapeado a un Servlet, así que solo se ejecutaría en algunos servlet no en todos
- validar sessión es mejor con filter, porque podríamos tener servlet públicos en los que no se necesita una autenticación

Al implementar, importar que sea de ==**jakarta.servlet**== porque hay muchos Filter. Es obligatorio implementar el contrato el método **doFilter()** los otros no.

ServletRequest es más genérico
HttpServletRequest 


> [!bug] error recurso no disponible
> revisar el pom
>   `<groupId>org.cr7.apiservlet.webapp.session</groupId>`
  `<artifactId>webapp-session</artifactId>`

si queremos privatizar multiples páginas a la vez
@WebFilter({"/cart/*"})

> [!info] LOGS
> Si queremos ver el log sin haber configurado nada, los log se encuentran en apache: /home/mooc/Descargas/**apache-tomcat-10.1.43/logs** y más concretamente en el fichero **catalina.out**


