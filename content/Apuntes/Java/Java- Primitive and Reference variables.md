### Variable primitiva
es almacenada como valor de una variable, su referencia está relacionada con la variable. Su valor es concreto.
**Variable de referencia**
son siempre objetos en java. Una referencia de objeto es devuelta por el constructor de la clase (Name). Su valor es una referencia.
Contiene el tipo de la variable y un identificador creado por java.
`Name@4aa2983d` = *type Name y su identificador es 4aa2983d*
Definiendo **toString()** dentro de la clase del objeto podemos alterar este formato de print default, donde modificas cómo debería verse.
```JAVA
public class Name{
	private String name;
	
	private Name(String name) {
		this.name = name;
	}
	public String toString() {
		return this.name;
	}
// de esta manera imprimimos cualquier objeto que es una instancia de la clase Name con System.out.println cmd, el string devuelto por toString() is lo que se imprime.
}
```

```JS
//sin el método toString()
Name luke = new Nam("Luke");
System.out.println(luke); 
//Name@4aa2983d 

//con el método toString()
System.out.println(luke); || System.out.println(luke.toString());
// Luke
```

#### Variables primitivas
Java tiene 8 variables primitivas. SON DIRECCIONES EN LA MEMORIA
1. boolean (true o false)
2. byte (contiene 8 bits, entre -128 y 127)
3. char (16-bit representado en un caracter)
4. short (16-bit representa un pequeño integer entre -32768 y 32767)
5. int (32-bit representa un mediano integer entre -2e32 y 2e31-1)
6. long (64-bit representa un gran integer entre -2e63 y 2e63-1)
7. float (floating-point number that uses 32 bits)
8. double (floating-point number that uses 64 bits)
Las variables tipo primitivo se pasan por valor no por referencia, así que si tenemos 
```
int first = 10;
int second = first;
int third = second;
sout( first + " " + second + " " + third ); // 10 10 10
sout( first + " " + second + " " + third ); // 10 5 10
```
#### Reference variables
Son todas las que no están en la lista de las 8.
Somos libres de crear nuestro propio tipo de variable mediante la definición de clases. **Cualquier objeto instanciado de una clase es una variable de referencia.**

La diferencia más significativa entre variables primitivas y de referencia es que los primitivos (generalmente números) son INMUTABLES.
```JAVA
Public clas Person {
	private String name;
	private int birthYear;
	
	public Person( String name ) {
		this.name = name;
		this.birthYear = 1970;
	}
	public int getBirthYear() {
		return this.birthYear;
	}
	public void setBirthYear( int birthYear ) {
		this.birthYear = birthYear;
	}
	public String toString() {
		return this.name + " ( " + this.birthYear + ")";
	}
}

```
```
public class Example {
	public static void main( String[] args ) {
		Person first = new Person( "First" );
		System.ou.println(first);
		youthen( first );
		System.ou.println(first);
		Person second = first;
		youthen( second );
		System.ou.println(first);
	}
	public static void youthen( Person person ) {
		person.setBirthYear(person.getBirthYear() + 1 );
	}
}

//First(1970);
//First(1971);
//First(1972);
```