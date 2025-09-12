> [!quote] Arquitectura
> **Vistas o frontend** -> Controllers -> Services
> **DAOs y Repo** -> BBDD
> **Servicios Rest** -> cloud/servers ==(Datos Externos)==
> **EJBs (Enterprise Java Beans), Obj remotos** -> cloud/servers ==(Datos Externos)==
> **WS Soap** -> cloud/servers ==(Datos Externos)==
> - No podemos hacer una prueba unitaria con las APIs. porque son remotas y no tenemos control sobre su comportamiento.
> - Nuestras clases tienen dependencias de estos, así que tenemos que probar un fragmento de código. Por ende, se aisla de todo ese código dependiente, simulando su función.
> - No tiene sentido hacer pruebas con esos datos. Por lo que tenemos que estar probando continuamente, y los datos van cambiando constantemente, por ende, la simulamos.

> [!tip] MOCKITO
> Es un framework que nos permite crear **objetos** simulados y dar un comportamiento deseado a un entorno controlado y determinado.  

> [!SUCCESS] 3 CONDICIONES DE NUESTRO ENTORNO DE PRUEBA
> - GIVEN = es la simulación, preparamos nuestro contexto, deps, inputs
> - WHEN() = invocamos el método con todos los comportamientos simulados
> - THENRETURN() = validamos (assertions, verify y spy que es un hibrido entre un mock y un obj real)
> - assertTrue, assertEquals(expected, real)
> - verify() = verifica si se llama al método dentro de un if(). Funciona porque hemos puesto **import static org.mockito.Mockito.*;**

```xml
<!--    mockito-core como mockito-junit-jupiter tienen que tener la misma version-->
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
            <version>3.6.28</version>
        </dependency>
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-junit-jupiter</artifactId>
            <version>3.6.28</version>
        </dependency>
```
- El Optional contiene una representación del Objeto para evitar el NPE.
- Es un wrapper porque envuelve al objeto, y tiene una ref. vacía de este obj

> [!summary] SHORTCUTS
> ALT + ENTER = Crear test
> ALT + INSERT = Override, Getters and Setters, Constructor
> ALT + INSERT = Importa la clase (List, Arrays, etc)
> ALT + INSERT = SetUp Method (==@BeforeEach==)

> [!warning] Remember!
> - NO se pueden hacer mock de métodos static, private ni final, solo de public y default

- Llamar al método con args por su nombre y si lo creas es por su tipo y nombre
- anyLong() = arg para cualquier id

 repository = mock(ExamenRepositoryOtro.class); //static method 
 
 #### **Injección de dependencias y anotaciones**
##### **Forma manual o explicita**
 1. Tenemos que *habilitar* las anotaciones dentro del **@BeforeEach** con: 
	 - **MockitoAnnotations.openMocks(this);**
 2. **@Mock**= por cada clase crea la instancia
 3. **@InjectMock** = crea la instancia y luego inyecta vía constructor

> [!FAILURE] ATTENTION!
> NO SE PUEDE INSTANCIAR UNA INTERFAZ, se debe intanciar el tipo concreto no el genérico, la clase que implementa!

```java
class ExamenServiceImplTest {
// repository y preguntasRepository se inyectan en service
    @Mock
    ExamenRepository repository;
    @Mock
    PreguntaRepository preguntaRepository;

    @InjectMocks
    ExamenServiceImpl service;

    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this);
	}
```
##### Forma implicíta:
Mediante **@EntendWith(MockitoExtension.class)** encima de la clase de test, ya no tendríamos que habilitar nada.
```java
@ExtendWith(MockitoExtension.class)
class ExamenServiceImplTest {

    @Mock
    ExamenRepository repository;
    @Mock
    PreguntaRepository preguntaRepository;

    @InjectMocks
    ExamenServiceImpl service;

    @BeforeEach
    void setUp() {
    }
       
```
1. Habilita las anotaciones
2. Inyecta dependencias
3. ejecuta con la extensión de mockito
4. Importante tener en el pom su extensión: **mockito-junit-jupiter**

> [!SUMMARY] 3* CONDICIONES
> GIVEN = DADO EL ENTORNO DE PRUEBA (Annotations)
> WHEN = CUANDO EJECUTAMOS EL MÉTODO DE PRUEBA
> THEN = ENTONCES VALIDAMOS CON ASSERTIONS O VERIFY
