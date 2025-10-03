1. se crea la clase 
2. se añade al persistence.xml dicha clase
3. la anotación @OnetoOne es la dueña de la ralación, en nuestro caso ClientDetail. La que llevará la FK
4. la clase Client sería nuestra parent, la que crearemos y persistiremos el Client con su detalle. (mappedby, cascade, orphanRemoval)
5. @JoinColumn solo va en el lado del dueño
6. cascade, hace que se guarde el detail también
7. OrphanRemoval: ayuda a eliminar el detail, de otra manera no se podría eliminar

@JoinColumn(name = "detail_id")
	“En la tabla clients, crea una columna llamada detail_id que apunta al id de client_details.”
	
El inverso no tiene la FK, solo sabe de su relación gracias al dueño.

+-----------------------+          +---------------------------+
|       clients         |          |      client_details       |
+-----------------------+          +---------------------------+
| id (PK)                 |           | id (PK)                     |
| name                  |           | phone_number        |
| last_name           |           | address                   |
| detail_id (FK) ---- +------> | id                           |
+-----------------------+          +---------------------------+

Relación:
- Client tiene una referencia a ClientDetail mediante "detail_id"
- ClientDetail no tiene ninguna FK, pero puede tener el atributo "client" con mappedBy
- Client es el dueño (tiene @JoinColumn)
- ClientDetail es el inverso (tiene mappedBy)

Clases:
```
class Client {
    @OneToOne(cascade = ALL, orphanRemoval = true)
    @JoinColumn(name = "detail_id")     // 🔥 Este lado tiene la FK
    private ClientDetail detail;
}

class ClientDetail {
    @OneToOne(mappedBy = "detail")      // 👈 Relación inversa
    private Client client;
}
```

#### Fetch Type Lazy e Eager

@OneToMany, @ManyToMany por defecto es FetchType.LAZY (terminan en Many)
@ManyToOne @OneToOne por defecto FetchType.EAGER (terminan en One)

Con EAGER, no da error si hacemos una consulta después del close()
FETCH en la createQuery: pobla, y siempre va después del LEFT JOIN.
LEFT JOIN : enlaza tablas, siempre haciendo referencia al atributo ej: c.detail
INNER JOIN: si no tiene el obj, da error "no entity found for query" , y con LEFT es ok.
