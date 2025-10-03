#### Comparing the equality of objects (equals) 
 
 The equals method first compares ==**if the addresses are equal**.== if so, the objects are equal. After this, we examine ==**if the types of the objects are the same**==: if not, the objects are not equal. Next, the Object-type object passed as the parameter is converted to the type of the object that is being examined by using a **==type cast==**, so that the values of the object variables can be compared.
```java
public boolean equals(Object compared) {
        // if the variables are located in the same position, they are equal
        if (this == compared) {
            return true;
        }

        // if the type of the compared object is not SimpleDate, the objects are not equal
        if (!(compared instanceof SimpleDate)) {
            return false;
        }

        // convert the Object type compared object
        // into a SimpleDate type object called comparedSimpleDate
        SimpleDate comparedSimpleDate = (SimpleDate) compared;

        // if the values of the object variables are the same, the objects are equal
        if (this.day == comparedSimpleDate.day &&
            this.month == comparedSimpleDate.month &&
            this.year == comparedSimpleDate.year) {
            return true;
        }

        // otherwise the objects are not equal
        return false;
    }
```


> [!summary] .equals()
> public ***boolean*** equals(Object compared) { 
> 1. this == compared ***referencias***
> 2. if!(compared instanceof **Object (la clase)**) ***doble negativo***
> 3. Obj class = (Obj) class ***castear***
> 4. if***comparar*** todos sus ***atributos*** y si tiene otro obj también sus atributos)

2. **Comparar referencias**
3. **Verificar tipo del objeto**
4. **Hacer casting**
5. **Comparar atributos**
## 📌 Extra: plantilla 

```java
@Override
public boolean equals(Object compared) {
    if (this == compared) return true;
    if (!(compared instanceof Clase)) return false;

    Clase other = (Clase) compared;

    // Comparar atributos aquí
    return this.a == other.a
        && this.b.equals(other.b);
}
```

##### Diferencias del punto 1 - referencias
- **if (this == compared) return true;**
“Si los dos objetos son exactamente el mismo en memoria, ya sé que son iguales → devuelvo true y salgo del método.”
```java
Bird red1 = new Bird("Red");
Bird red2 = red1; // Mismo objeto exacto

System.out.println(red1.equals(red2)); // → entra al if y se sale ahí mismo
```
🔹 Aquí this == compared es true → se ejecuta return true; → 🚪 el método se termina en ese momento.
##### Diferencias del punto 2 - verificación del tipo de objeto
- En caso de : **if (!(compared instanceof Clase)) return false;** ***(-)***
“Si lo que me estás comparando NO es un pájaro, digo que NO son iguales.”
Comparar un pájaro con un coche →  no tiene sentido → devuelvo false.
==*“Primero, asegurémonos de que los dos sean del mismo tipo de juguete.*==
==*Si no, ni me molesto en compararlos.”*==

- En caso de : **if ((compared instanceof Clase)) return true;** ***(+)***
“Si me estás dando otro pájaro, digo que ya son iguales, sin mirar si tienen el mismo nombre.”
"Red" y "Blue" son pájaros → entonces esta versión diría que son iguales, ¡aunque tienen nombres distintos!


> [!warning] Return true;
> Cuando escribes **return true;** , le estás diciendo a Java:
“¡Listo! Ya tengo la respuesta: sí, son iguales. No sigas leyendo más código.”
Así que todo lo que venga después en el método equals() ya no se ejecuta.
*El método termina ahí mismo.*

> [!exemple]
```java
 Bird a = new Bird("Red");
 Bird b = new Bird("Blue");
 System.out.println(a.equals(b));  // → true 😱 (¡aunque tienen nombres distintos!)
```
> Esto pasa porque el método solo ve que b es un Bird y automáticamente dice "sí, son iguales", sin mirar el nombre.

### What is Object? ...
The contains method of a list uses the equals method that is defined for the objects in its search for objects. In the example above, the Bird class has no definition for that method, so a bird with exactly the same contents — but a different reference — cannot be found on the list.
Example
```java
public class Bird {
    private String name;

    public Bird(String name) {
        this.name = name;
    }

    public boolean equals(Object compared) {
        // if the variables are located in the same position, they are equal
        if (this == compared) {
            return true;
        }

        // if the compared object is not of type Bird, the objects are not equal
        if (!(compared instanceof Bird)) {
            return false;
        }

        // convert the object to a Bird object
        Bird comparedBird = (Bird) compared;

        // if the values of the object variables are equal, the objects are, too
        return this.name.equals(comparedBird.name);

        /*
        // the comparison of names above is equal to
        // the following code

        if (this.name.equals(comparedBird.name)) {
            return true;
        }

        // otherwise the objects are not equal
        return false;
        */
    }
}
```

- Con primitivos, el operador == compara el valor.
 - Con objetos, el operador == compara si son el mismo objeto en memoria, no el contenido.

| Código                                                           | ¿Qué hace?                                            | ¿Correcto?                 |
| ---------------------------------------------------------------- | ----------------------------------------------------- | -------------------------- |
| `this.getName() == comparedBook.getName()`                       | Compara **referencias de String** (¡No el contenido!) | ❌ No                       |
| `this.getName().equals(comparedBook.getName())`                  | Compara el **contenido del texto**                    | ✅ Sí                       |
| `this.name.equals(comparedBook.name)`                            | También compara el contenido (acceso directo)         | ✅ Sí, si `name` es visible |
| `this.getPublicationYear() == comparedBook.getPublicationYear()` | Compara números (`int`)                               | ✅ Sí                       |

