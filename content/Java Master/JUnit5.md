1. JUnit Jupiter: API para escribir nuestros tests. Permite escribir extensiones. 

> [!summary] Annotations TEST
> - \@Test : para escribir ejecutar métodos en folder test por cada método
> - \@DisplayName: para describir la función del método, como un nombre
> - \@Nested: Inner class, anidada
> - \@Tag, 
> - \@ExtendWith, 
> - \@BeforeEach: después de que se ejecute una tarea
> - \@AfterEach: después de ejecutarse una tarea
> - @BeforeAll: se ejecuta una vez al inicio y es *static*(recursos, conn)(clase principal)
> - @AfterAll: se ejecuta una vez al final y es *static* (clase principal)
> - @Disable: para deshabilitar el método, se lo salta. 
> - @EnabledOnOs(OS.WINDOWS)
> - @EnabledOnOs({OS.LINUX, OS.MAC})
> - @EnabledOnJre(JRE.JAVA_17)
> - @DisabledOnJre(JRE.JAVA_17)
> - @DisabledIfSystemProperty(named = "java.version", matches = ".\*15*.")
> - @EnabledIfSystemProperty(named= "ENV", matches = "dev")
> -- 
Se crea una nueva instancia por defecto por cada uno de estos en la clase test.

> [!quote] variable de ambiente
> Se edita la configuración de Run test y se le añade -DENV=dev para que cree el entorno. Quedaría así: -ea -DEV=dev
> El -D significa que se configura un System Property


> [!hint] Ejecutar este método para ver las props de Java
```java
 void imprimirVariablesAmbiente() {
     Map\<String, String> getenv = System.getenv();
     getenv.forEach((k, v) -> System.out.println(k + " = " + v));
 }
```

1. JUnit Vintage
2. Otros Testing framework
3. fail(): ejecuta el método donde se encuentre con error.

Los métodos pueden pasar la prueba pero para asegurarse es mejor ejecutarlos en la clase, con los demás métodos.

```java
@Nested
class SystemProperties{
    @Test
     void imprimirSystemProps(){
         //bucle del props del sistema
     }
    @Test
    @EnabledIfSystemProperty(named="", matches=".*15.*"){
    void testJavaVersion(){
    }
    ...
}
```