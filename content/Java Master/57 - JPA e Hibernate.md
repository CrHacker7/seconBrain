- ==**@Entity**== -> indica que la clase es de hibernate o JPA, está asignada a una tabla. Está mapeada la clase con una tabla del mismo nombre de la clase. 
- Si omitimos **==@Table==**, se asume que la tabla se llama igual que la clase.
```java
@Entity
@Table(name = "clients") //optional
public class Client {}
```

- **==@Id==** -> se mapea a un campo id, con el mismo nombre del atributo.
- ==**@GeneratedValue(strategy = GenerationType.IDENTITY)**== -> para que el motor de sql maneje la id de forma automática.
```java
	@Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
```

- Si le queremos poner un nombre distinto de la clase y el campo de la tabla se le pone el ==**@Column(name="another_name")**==
```java
	@Column(name = "payment_type")
    private String paymentType;
```
- Las tablas se relacionan con FK(Foreign Key)
- Siempre tiene que existir un constructor vacío, y sí tenemos alguno con params, debemos añadir uno sin params. De esta manera JPA crea las instancias con el new.
- 
**EntityManager** -> es un admin de clases entities que nos permite implementar un CRUD.

-  En vez de devolver campos devuelve OBJETOS. 
`*
```java
  public static void main(String[] args) {
	  EntityManager em = JpaUtil.getEntityManager();
	  em.createQuery("SELECT c FROM Client c", Client.class)
	  //Al objeto Client le llamamos `c` FROM la CLASE `Client` y le damos un nombre al objeto `c
  }
  ``` 
 
Esto nos devuelve una lista de clientes:
```java
List<Client> clients = em.createQuery("SELECT c FROM Client c").getResultList();
```

- **Claúsula WHERE HQL **
```java
public static void main(String[] args) {
	EntityManager em = JpaUtil.getEntityManager();
	Query query = em.createQuery("SELECT c FROM Client c WHERE c.paymentType=?1", Client.class);
	//Primero va el signo pregunta luego el num de arg. SIrve para buscar cualquier campo EXCEPTO el Id
```
**Segunda forma de WHERE**
- Con esta manera el find guarda en la memoria del contexto de hibernate y la segunda vez ya no va a la bbdd sino que la recupera desde la memoria, como un caché. Guarda el obj en la sesión. Tiene que ser el mismo id o consulta.
```JAVA
Client client = em.find(Client.class, id);
//Con find() siempre busca por la llave primaria, el id.
```
- Para guardar ==em.getTransaction().**persist(c)**== un objeto en el EntityManager tiene que estar entre el ==em.getTransaction().**begin()**== y el ==em.getTransaction().**commit()**==
```java
			em.getTransaction().begin();

            Client c = new Client();
            c.setName(name);
            c.setLastname(lastname);
            c.setPaymentType(payment);
            
            em.persist(c);
            
            em.getTransaction().commit();
```

**Esqueleto para Crear objeto**
```JAVA
public static void main(String[] args) {

        EntityManager em = JpaUtil.getEntityManager();
        try {
//ask for new data        
        String name = JOptionPane.showInputDialog("Enter a name:");
            String lastname = JOptionPane.showInputDialog("Enter a lastname:");
            String payment = JOptionPane.showInputDialog("Enter a payment method:");
//create objet
		  Client c = new Client();
		  c.setName(name);
		  c.setLastname(lastname);
		  c.setPaymentType(payment);
            
//begin            
            em.getTransaction().begin();
            
//save
            em.persist();
//commit            
            em.getTransaction().commit();
//optional for visualizing new data
			System.out.println("The registered client's ID is " + c.getId());
            c = em.find(Client.class, c.getId());
            System.out.println(c);            
        } catch (Exception e) {
//rollback
            em.getTransaction().rollback(); 
//printTrace            
            e.printStackTrace();
        }finally {
//close        
            em.close();
        }
    }
```

**Esqueleto para Modificar**
1. obtener el EntityManager
2. pedir id
3. buscar id en bbdd y hacer consulta con find
4. obtenemos client y mostramos sus datos en un form tipo JOptionPane
5. modificar datos del formulario con JOptionPane.showInputDialog("E")
6. begin()
7. pasamos datos
8. hacer merge (actualiza los datos del obj en el "context persistence")
9. commit (actualiza el update)
10. imprimir datos
11. rollback
12. close

**Diferencias para buscar el id**

| Aspecto        | `Client c = em.find(Client.class, id);` | `c = em.find(Client.class, c.getId());`                    |
| -------------- | --------------------------------------- | ---------------------------------------------------------- |
| ¿Qué busca?    | Busca un `Client` con `id` dado         | Busca un `Client` con el ID de un objeto `c` que ya existe |
| ¿Qué necesita? | Solo el `id`                            | Que `c` ya esté inicializado                               |
| ¿Útil cuándo?  | Primera vez que accedes al objeto       | Si quieres refrescar la instancia desde la BD              |
**Esqueleto para Eliminar**
```java
		Scanner sc = new Scanner(System.in);
        System.out.println("Enter the client's ID to be deleted ");
        Long id = sc.nextLong();
        EntityManager em = JpaUtil.getEntityManager();

        try {
            Client client = em.find(Client.class, id);

            em.getTransaction().begin();
            em.remove(client);
            em.getTransaction().commit();

        } catch (Exception e) {
            em.getTransaction().rollback();
            e.printStackTrace();
        }finally {
            em.close();
        }
```
 
1. buscar client en la bbdd por el id y lo obtenemos
2. comenzamos la transacción begin()
3. dentro de la transacción eliminamos con remove(cliente)
4. actualizamos con commit()
Para poder eliminar con remove se tiene que manejar mediante el contexto de JPA, o sea con el método find()