### Config file de idiomas props (Resource Bundle)
1. En la clase ProducerResources, añadir método que retorne bean ResourceBundle con la anotación @Named("msg")
2. main -> resources -> new resource bundle con nombre 'messages' creamos el diccionario, y añadimos 2 files; messages_en, messages_es
3. En el index reemplazamos los valores de los botones, labels, input, output, navbar, etc. por las claves del messages.prop. (separado con punto)  `<h:commandButton value="#{msg['product.button.create']}"`

> [!warning] @Produces
> - Inyectar un tipo que no puedes anotar con scopes del CDI.
> - Una clase de terceros, Objetos de terceros, clases no anotadas, lógicas especiales de instancia, se necesitará @Produces

| Situación                                      | ¿Necesita `@Produces`? | Motivo                                                |
| ---------------------------------------------- | ---------------------- | ----------------------------------------------------- |
| Clase propia con `@ApplicationScoped`, etc.    | ❌ No                   | CDI ya la gestiona                                    |
| Quieres inyectar el resultado de un método     | ✅ Sí                   | CDI necesita saber cómo crear ese valor               |
| Clase de terceros sin soporte CDI directo      | ✅ Sí                   | Tú tienes que decirle al contenedor cómo instanciarla |
| Clase de terceros gestionada por el contenedor | ❌ No                   | Ya es inyectable sin más                              |

---
- 📅 Date : 2025-10-02
- 💬 Commit description : Configuration messages.properties with Resources Bundle
- 🔗 Commit :[{{commit_hash}}](https://github.com/CrHacker7/javaMaster/commit/{{93c1873629ed73ea3cbc4a518075ddb97c1a51c2}})
---
##### Implementando controlador para cambiar el idioma
1. Crear clase LenguageController con @SessionScoped, este obj se tiene que serializar en el request, por ende implements Serializable y añadimos la constante: `private static final long serialVersionUID = 12344321L;`
2. Crear constructor con la anotación **@PostConstruct**, así cada vez que se crea el obj en la sesión http, se tiene que inicializar. Y no en el constructor que a veces no puede tener acceso a recursos porque todavía no han sido inicializados. Por ej. los @Inject y todas las deps se inyectan posteriormente, entonces dentro del contexto del **@PostConstruct**, sí estarán disponibles todos los componentes inyectados.

> [!tip] @PostConstruct
> Cuando se trabaja con CDI es mejor iniciar el componente en un método anotado con @PostConstruct, y una vez que se hayan inyectado todas las deps, y evitar el constructor.

4. Inicializar los lenguages soportados y la localización en el método init()

> [!tip] init()
> localización = `FacesContext.getCurrentInstance().getViewRoot().getLocale();`
> `suppLangs = new HashMap(); `
> `suppLangs.put("Spanish", "es"); `
> `suppLangsput("Spanish", "es")`

4. Crear método select() para hacer el switch del lenguaje con arg de la clase ValueChangeEvent, iteramos, preguntamos si es igual al nuevo 

> [!tip] select()
> Convertir a string el nuevo lenguage
> iteramos el map, si igual al nuevo; iniciamos y seteamos. `FacesContext.getCurrentInstance().getViewRoot().setLocale(this.locale);`

5. Añadimos el flag locale en main.xhtml y modificamos tag html a `f:view`
---
---
- 📅 Date : 2025-10-02
- 💬 Commit description : Implementing a language switch controller
- 🔗 Commit : [{{commit_hash}}](https://github.com/CrHacker7/javaMaster/commit/{{29668902f32294d6a8379031335a6ccf90f17cd5}})
---

#### Utilizando e inyectando los textos de idioma en el controller
`facesContext.addMessage(null, new FacesMessage(String.format(bundle.getString("product.message.edit"), product.getName())));`

---
---
- 📅 Date : 2025-10-03
- 💬 Commit description : Adding dictionary for flash messages - validations
- 🔗 Commit : [{{commit_hash}}](https://github.com/CrHacker7/javaMaster/commit/{{7f99bacd7e06f7993449b6f729702dd9095e6120}})
---

### Sección 73 - JSF3 - PrimeFaces
ctrl+shft+alt+j = selecciona todos los tag del mismo tipo


> [!warning] REPOSITORY
> **CrudRepository\<Product>** -> **ProductRepository** extends CrudRepository\<Product>(findByName) -> **ProductRepositoryImpl** 

El **ProductServiceImpl** implements **ProductService**, también debe tener inyectado el **ProductRepository** para obtener el findByName.
