## Inicio proyecto
- ***ng new 03-gifs-app --no-standalone***
- Copiar el CDN de Bootstrap
- Pegarlo en el head del index.html y guardar
- Borrar todo el contenido de app.component.html
- Poner "hola mundo" como ejemplo y  `ng serve -o`
## Diseño y estructura inicial
- Navbar y sidebar nunca van junto al resto de código ya que hacen funciones distintas, así que los ponemos en un folder aparte. Agruparlo por funcionalidad
- `ng g m gifs`  genera un módulo dentro de gifs folder
- `ng g c gifs` genera un component
- `ng g m shared`
- No se hace la importación automática de los módulos. 4
	- Un MODULE importado en otro MODULE,MODULE va en IMPORTS 
- Creamos un sidebar y Exportalo porque queremos usarlo en la página principal. Los módulos se exportan, escribimos `export: [SidebarComponent]` . Esto está en nuestro module.ts principal.
## Diseño de Gifs
> [!tip] 
> Para desactivar el modo compactfolder, que es que los folders no se distribuyen en cascada, 'CTRL' + '+' , settings, escribir "explorer.compactfolder" y desactivar.

- "a-component" escribe todo el código .ts creado manualmente.
- El apartado de  **declarations** del gifs.module.ts son para usar las componentes que están dentro de ese module, en nuestro caso será para todas las de Home-page
## @ViewChild - Referencia al HTML
- Nos sirve para tomar una referencia local, es decir en el HTML. Se distingue porque empieza por **#** y que daría así:
```java
@Component({
    selector: 'gifs-search-box',
    template: `
	<input type="text"
            class="form-control"
            placeholder="Buscar gifs..."
            (keyup.enter)="searchTag()"
            #txtTagInput 
        >`
})

export class SearchBoxComponent {
    @ViewChild('txtTagInput')
    public tagInput! : ElementRef<HTMLInputElement>;
//! es not null
    constructor() { }

    searchTag() {
        const newTag = this.tagInput.nativeElement.value;
        console.log({newTag});
    }
}
```
- El @ViewChildren es un array, hace referencia a más de input
## GifsService
- Creado manualmente: click derecho en folder services, new file, **gifs.services.ts**, **a-service** crea todo el file de 0.
- Un service para inyectarlo es en el constructor.

> [!tip] Cajas cuadradas moradas son métodos y cajas rectangulares azules son propiedades.

- con | podemos hacer que lo que escribamos se vea como queremos sin modificar la data real. Ej. 
```java
<button *ngFor="let tag of tags" class="list-group-item list-group-item-action">
            {{tag | titlecase}}
            //también hay uppercase todo en MAYUSCULAS
```

## Giphy API Key- Giphy Developers
1. crear cuenta
2. seleccionar API
3. añadirlo a gifsService `public gifsService
4. en Postman
5. private apikey: string = 'OampTUt7ZiXdqPrmt1Kgk7Qw;
6. concatenamos
api.giphy.com/v1/gifs/ + search + ? + api_key= + secretKey + &+ q + =+ loquesea

| key     | value                    |
| ------- | ------------------------ |
| api_key | slejr4lgi58...
| q       | valorant                 |
| limit   | 10                       |
## Realizar una petición HTTP
- Ponemos todo en un fetch que regresa una promesa.









- lidia.lopez@gentcat.cat
