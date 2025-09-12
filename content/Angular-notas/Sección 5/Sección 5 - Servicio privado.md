1. Cambiamos el constructor de la clase main.ts (padre )de `public` a `private.`
2. Creamos un `getter` para **characters** que (100pre) regresa un valor. Pasamos por el servicio para obtener la data de los characters, ya que se encuentran allí.
> si modificamos los character, se modificará de nuestro servicio, para evitarlo, copiamos cada personaje con el spread
		`get characters(): Character[] {`
			`return [...this.dbzService.characters ];`
		`}`
3. Cambiamos el main.html y solo ponemos characters porque ya está accesible. Ya no tenemos que poner dbzService. por delante de characters.
4. Con los eventos, se crea el método en el padre
	  `onDeleteCharacter( id: string ): void {`
         `this.dbzService.deleteCharacterById( id );`
	  `}`
	Y cambiamos el main.html con el nombre del método que acabamos de crear.
5. 	
