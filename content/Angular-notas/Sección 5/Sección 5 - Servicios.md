>Aquí está la lógica de negocio. Donde se manejarán los datos que tenemos en nuestro **component.ts padre** 
 Si el documento está vacío ponemos ==`a-service + tab`== y cambiamos su nombre a **DbzService.ts**

  >Copiamos todo al DbzService.ts (clases y métodos ). El main-page.component.ts se queda vacío. 
  
  En el main-page.component.ts hacemos el constructor:
  `constructor( public debService : DbzService ) {`
        
    `}`
>En el main-page.component.html no reconocerá nada porque lo hemos cambiado, para volver a cogerlo ponemos ==dbzService==. por delante de nuestras clases y métodos

- Instalar ==`npm i uuid`==
- Importar en service.ts
	- `import {  } from 'uuid';` 
- si sale error uuid, instalamos dependencias para desarrolladores 
	- ==`npm i --save-dev @types/uuid`== 
- si aún así no se quita el error 
	- en la paleta de cmd   **>reload window**

