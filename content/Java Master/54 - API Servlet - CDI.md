1. Se añade la dependencia en el pom
2. Se añade las anotaciones @SessionScoped arriba de la clase Cart y @Named para que coja la clase Cart pero en minúsculas como componente y tiene que tener el constructor vacío SÍ O SÍ
####  ¿Por qué necesitarías un constructor vacío?
	- Para crear un objeto "vacío" temporalmente y llenarlo luego con setters.
	- Para trabajar con frameworks como:
	- JPA/Hibernate (necesitan un constructor vacío para instanciar entidades).
	- Jackson o Gson (al deserializar JSON).
	- Spring (cuando inyecta dependencias o maneja modelos en formularios).

> [!warning] Reglas típicas de un JavaBean
> 1. Tener un constructor público sin argumentos
> 2. Tener propiedades privadas con métodos getter y setter públicos
> 3. Ser serializable (opcional, pero común)
> 4. añadir el beans.xml en el WEB-INF : `<beans />`
#### Configurar el beans.xml
Copiar el beans de la pág. jakarta.ee
https://jakarta.ee/specifications/cdi/3.0/jakarta-cdi-spec-3.0.pdf 

```xml
<beans xmlns="https://jakarta.ee/xml/ns/jakartaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
https://jakarta.ee/xml/ns/jakartaee/beans_3_0.xsd"
version="3.0">
```

> [!warning] @Produces (se importa de jakarta.enterprise.inject)

Antes de inyectar debemos añadir el scoped de la anotación encima de la clase, @ApplicationScoped, @SessionScoped
Hay 3 maneras de inyectar:
1. **Mediante constructor**
```
	@Inject
	public CategoryRepositoryImpl(@Named("conn") Connection conn) {
        this.conn = conn;
    }
```
2. **Mediante atributo**
```
	@Inject
    @Named("conn")
    private Connection conn;
```
3. **Mediante setter**

Se tiene que inyectar en la Interfaz para que si nuestro código cambia, lo podamos manejar fácilmente y se quede más desacoplado.

- si tenemos multiples ProductServiceImpl, le tenemos que poner la anotación @Alternative a todos salvo a uno que será el usado por defecto.
- Solo se inyecta el que tenga la anotación @Named()

Volver a mirar 

#### Ciclo de vida de los beans con anotaciones @Postconstructor y @Predestroy
```java
    @Inject
    private transient Logger logger;
//  Cause the Logger is not serializable, we can not keep it as a session, that's why we use the modifier
//  "transient" when the class implements serializable && when it is @ConversationScoped or @SessionScoped
//  Logger is not part of the session of the Cart, so we can inject to the cart and we use it for printing data in logs.
```


> [!warning] Stereotype
> - Permite crear tus propias anotaciones personalizadas que combinan otras anotaciones de CDI o Jakarta EE.
> - Sirve para evitar escribir repetidamente combinaciones de anotaciones comunes en tus beans CDI.
```java
"CLASE"
@Named
@SessionScoped
public class Cart implements Serializable {
    ...
}
------------------------------------------
"DEFINIMOS NUESTRA CLASE COMO ANOTACIÓN"
@Stereotype
@Named
@SessionScoped
@Retention(RUNTIME)
@Target(TYPE)
public @interface CartBuy {
}
---------------------------------------------
"APLICAMOS EN UNA CLASE"
@CartBuy
public class Cart implements Serializable {
    ...
}
```

Todas las anotaciones que creemos como clase, tienen que tener la anotación más importante, ==@InterceptorBinding==
- Si hay más de un valor en el @Target poner entre llaves!
```java
@Target({ElementType.METHOD, ElementType.TYPE})
```
- NO OLVIDAR en el beans.xml añadir el paquete y la clase
```xml
<interceptors>
        <class>
            org.cr7.apiservlet.webapp.cookie.interceptors.LoggingInterceptor
        </class>
    </interceptors>
```

- La anotación @AroundInvoke siempre es de tipo Objeto
```java
  @AroundInvoke
    public Object transactional(InvocationContext invocation) throws Exception {
    //commit...
        Object result = invocation.proceed();
        return result;
	//catch rollback...
    }
```
**¿Qué es InvocationContext?**
Es un objeto que te da información sobre el método interceptado:
**getMethod()** → método que se está ejecutando.
**getTarget()** → el objeto (bean CDI) sobre el que se invoca.
**getParameters()** → los parámetros del método.
**proceed()** → continúa con la ejecución normal del método original.

****¿Qué es InjectionPoint**
ip.getMember()               // Método o campo donde se inyecta
ip.getMember().getName()     // Nombre del campo
ip.getMember().getDeclaringClass() // Clase donde ocurre la inyección
ip.getAnnotated().getAnnotations() // Anotaciones presentes
```java
@Produces
public MyType createSomething(InjectionPoint ip) {
    ...
}
```

#### ¿Cuál es la diferencia con InjectionPoint e InvocationContext?
| Aspecto         | `InjectionPoint`                            | `InvocationContext`                              |
| --------------- | ------------------------------------------- | ------------------------------------------------ |
| Contexto        | Inyección de dependencias (`@Inject`)       | Interceptación de métodos (`@Interceptor`)       |
| Se usa en       | Métodos `@Produces`                         | Métodos `@AroundInvoke`                          |
| Qué describe    | El lugar donde se está inyectando un objeto | El método que se está ejecutando/interceptando   |
| ⚙️ Casos de uso | Crear beans personalizados                  | Agregar lógica antes/después de ejecutar métodos |
