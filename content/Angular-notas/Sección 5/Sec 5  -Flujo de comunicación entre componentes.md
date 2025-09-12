>En el hijo se pone un decorador ==@**Input()**== le decimos al ListComponent que puede recibir una property characterList. Se puede cambiar el nombre de la propiedad en los parentesis de @Input().

>El main-page.component.ts es el padre, así que los datos que hayan sobreescribiran los que tenga en el hijo, pero el hijo le dice que formato tendrá o qué valor por defecto aparece en caso que el padre no tenga datos.

>Mandar del padre al hijo, o queremos asignar mediante atributo, se hace mediante `[ ]`
>y si queremos escuchar los eventos es mediante `( )`

### Ejemplo de flujo

### Componente padre (main-page.componente.ts)
>1. Crear el método

 `onDeleteCharacter( index: number ) {`
    `this.characters.splice(index, 1); //borra un elemento a la vez`
`}`
### Componente hijo (list.componente.ts)

>1. Importar el  ==`@Output()`== y el ==`eventEmitter`== y añadimos el new objeto o emitter
>2. Definir el objeto eventEmitter 

`@Output()`
  `//dos maneras de hacerlo:`
  `//* public onDelete = new EventEmitter<number>();`
  `public onDelete: EventEmitter<number> = new EventEmitter();`

>Emitir el índice

  `//TODO: EMITIR EL ID DEL PERSONAJE`
  `onDeleteCharacter(index: number) :void {`
    `this.onDelete.emit(index);`

### Componente main-page.component.html
>Para escuchar eventos usamos Paréntesis. Ponemos el nombre de nuestro evento dentro que llama al eventEmitter, luego usamos llamamos al método y en su paréntesis ponemos ==$event== que es el valor emitido de ese elemento

`<dbz-list (onDelete)="onDeleteCharacter($event)"`  