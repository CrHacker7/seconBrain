
En el componente **.ts** se puede crear toda la página del código base. 
Gracias a los snippets poniendo `a-component + tab` nos saldrá todo.
Ahora se modifica el *==selector==* y el *==template==*, la *==clase==* y eliminar el *~~OnInit~~*

Los métodos se ven de color *lila* y las propiedades se ven en *azul*.
-Hay dos maneras de hacer un método:
	Getter: define un accesor que actúa como una propiedad, permitiéndome acceder al valor sin paréntesis.
	Método normal: define una función tradicional que requiere paréntesis para invocarse y puede incluir lógica más compleja.
#### Directivas ngIf- ngFor
Con `*ngIf + tab` si es usado en el html, podemos hacer que aparezca o no dentro de nuestra pag web.
`<button` 
    ==`*ngIf="name !== 'Spiderman'"`==
    `(click)="changeHero()"` 
    `class="btn btn-primary mx-2">`
    `Cambiar nombre`
`</button>`

Con ==ng-template== hace invisible por defecto, el tag html  
	<div *ngIf="deletedHero; else ==nothingWasDeleted==">         
	<ng-template  ==`#nothingWasDeleted`==>                         

>En else ponemos el nombre que queramos y lo usamos en la 
> Referencia Local "`#nothingWasDeleted`" del ng-template

#### Módulos de angular [[51-450]]

 Los **module.ts** sólo están disponibles dentro del folder que lo encapsula. Para que sea visible dentro de mi folder, lo usamos en `declarations; [ CounterComponent] `  Para que sea visible en otros modules, tenemos que `exports: [ CounterComponent ] ` 
 
>En los `imports: []` siempre van los **blablaModule**  

>ordenar orden alfabético: seleccionar lo que queremos ordenar
ctl + p -> palette
añadir ` > ` busca en los settings
Sort Lines Ascending 

>Los CommonModule son para exportar las Directivas ngIf, etc
Así que se ponen en el module general que he creado encapsulando mis dos componentes, uso un module para dos componentes. 
Y allí la `import: [ CommonModule] `

### Git
vuelve al último commit que hicimos -> `git checkout -- .` 