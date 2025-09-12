## Abstracción: [java](Java pdf2) 
Cuando hablamos de abstracción en lo que, a la programación orientada a
objetos, nos estamos refiriendo a la posibilidad de poder identificar el posible
comportamiento de un objeto para posteriormente convertirlo en sus métodos y
funciones dentro de una clase

Imagina que tienes una bicicleta. La bicicleta tiene diferentes cosas que puede hacer, como "moverse", "frenar" o "cambiar de velocidad". Pero tú no necesitas saber cómo se hace todo eso en detalle (por ejemplo, cómo funcionan los engranajes dentro del sistema de frenos). Solo te interesa que la bicicleta se mueva cuando pedaleas, que frene cuando aprietas el freno, etc.

En programación, cuando hablamos de abstracción, estamos haciendo algo similar. Es como si tomáramos la bicicleta y dijéramos: "Voy a usarla, pero no me importa cómo funciona por dentro, solo sé que tengo estos comportamientos (moverse, frenar, etc.) que puedo usar".

En el mundo de la programación, la abstracción significa que podemos crear objetos (como la bicicleta) con comportamientos definidos (como moverse o frenar), pero no necesitamos preocuparnos por los detalles internos de cómo funcionan esas acciones. Los detalles internos se esconden para que solo trabajemos con lo que necesitamos.

En términos de programación orientada a objetos, esto se hace cuando creamos una clase (como un plano para hacer bicicletas) y dentro de esa clase definimos los comportamientos como métodos (moverse, frenar, etc.). Y luego, cuando usamos la bicicleta, solo necesitamos llamarlos y no preocuparnos por cómo funcionan por dentro.

¡Espero que ahora sea más claro! 😄

## Encapsulamiento
Imagina que tienes una caja. Dentro de esa caja, hay muchas cosas: una pelota, una cuerda, un juguete. Sin embargo, tú no puedes ver lo que está dentro de la caja simplemente mirándola desde afuera. Tienes que abrir la caja para ver qué hay dentro. Pero, lo importante es que tú decides qué se puede ver y qué no. Por ejemplo, puedes decidir que solo la pelota pueda salir de la caja, pero la cuerda y el juguete están guardados, así que nadie puede tocarlos sin que tú se lo permitas.

Ahora, en programación, el encapsulamiento es algo parecido. Piensa en un objeto (como la caja) que tiene información y comportamientos internos (como la pelota, la cuerda, el juguete). Usamos el encapsulamiento para proteger esa información y asegurarnos de que otras partes del programa no cambien o accedan directamente a los detalles internos sin permiso.

**En términos simples:**
La caja es el objeto.
La pelota, la cuerda y el juguete son los datos y los detalles internos.
Abrir la caja es usar métodos (funciones) que permiten acceder o modificar esos datos de una manera controlada, sin que el exterior pueda tocarlos directamente.
Esto ayuda a que todo funcione de manera ordenada y segura, sin que se cambien cosas que no deberían tocarse. Por ejemplo, si tienes una caja con una pelota, tal vez quieras que la pelota solo se pueda sacar si alguien te pide "¡déme la pelota!" (a través de un método). De esta forma, nadie puede simplemente tomarla de la caja sin pedirlo.
El encapsulamiento ayuda a que todo esté ordenado y protegido. 😊

## Modularidad
Imagina que estás construyendo una casa con bloques de construcción, como los de Lego. Cada bloque es una parte que hace algo muy específico, como una pared, una puerta o una ventana. Cuando construyes tu casa, puedes hacerla más fácil de construir si decides usar muchos bloques pequeños que tengan una función o propósito claro.

**Por ejemplo:**
Un bloque puede ser una pared.
Otro bloque puede ser una puerta.
Otro bloque puede ser una ventana.
Lo genial de esto es que cada bloque es independiente y puedes usarlo de nuevo en otra construcción si lo necesitas. Si algo se rompe, solo tienes que cambiar ese bloque, sin tener que destruir toda la casa.

Ahora, en programación, la modularidad funciona de manera similar. En lugar de tener un solo programa gigantesco, lo dividimos en módulos pequeños. Cada módulo tiene una función específica y hace solo una cosa, como los bloques de Lego que tienen una función clara (como ser una pared o una ventana).

**¿Por qué es útil la modularidad?**
Facilita el trabajo en equipo: Si varias personas están trabajando en un proyecto, cada una puede trabajar en diferentes bloques sin chocar con lo que otros están haciendo.
Reutilización: Si ya tienes un bloque que hace algo muy bien, lo puedes usar de nuevo en otro proyecto sin tener que crear uno desde cero.
Mantenimiento fácil: Si algo no funciona bien en tu casa de Lego, solo cambias el bloque que está roto. Lo mismo pasa con el código: si un módulo no funciona bien, solo cambias ese módulo sin tocar todo el programa.
**En resumen:**
La modularidad significa dividir tu programa en pequeñas partes que hagan cosas específicas, y estas partes (o módulos) pueden trabajar juntas para formar algo mucho más grande. Esto hace que el código sea más ordenado, fácil de entender, y más fácil de mantener o mejorar.

## Ocultación (encapsulamiento de la información) 

**La historia de la caja secreta**
Imagina que tienes una caja secreta en tu casa. Esta caja tiene un botón en el exterior que, cuando lo presionas, hace que la caja haga algo interesante, como abrirse y mostrar algo dentro (puede ser un regalo o una sorpresa). Pero aquí está lo importante: fuera de la caja, no sabes cómo funciona exactamente el mecanismo por dentro. Solo sabes que al presionar el botón, la caja hace lo que quieres. No te importa cómo funciona internamente; solo necesitas saber qué hacer para que funcione.

**Por ejemplo:**
El botón es lo que ves fuera de la caja, lo que puedes usar.
Dentro de la caja hay piezas, mecanismos, cables y cosas que trabajan para que el botón haga su trabajo, pero no necesitas verlas ni saber cómo funcionan.
En programación, la ocultación funciona igual. Imagina que un objeto (como una caja) tiene cosas importantes (información o datos) dentro, pero solo quieres que otras personas interactúen con tu objeto de una manera simple. Así, no tienen que saber cómo funciona todo por dentro, solo tienen que saber cómo usarlo correctamente.

**Ejemplo:**
Supón que tienes una clase llamada Coche. Un coche tiene muchas cosas dentro: el motor, las ruedas, el combustible, y muchas más. Pero si te subes a un coche, solo necesitas saber cómo arrancarlo, cómo acelerar o cómo frenar. No necesitas saber cómo funciona el motor o cómo el sistema de combustible hace que el coche se mueva.

El coche es un objeto.
Las funciones como arrancar el coche o acelerar son los métodos que el objeto tiene.
Los detalles del motor y otros sistemas están ocultos, y tú solo puedes interactuar con el coche usando esos métodos.
**¿Por qué es útil la ocultación?**
**Simplicidad:** Ayuda a que los usuarios de tu código no se preocupen por detalles complejos que no necesitan saber. Solo necesitan saber cómo usar el objeto.
**Seguridad:** Si ocultas información, los usuarios no pueden modificar o hacer cosas que podrían dañar el funcionamiento de tu objeto.
**Facilidad de mantenimiento:** Si decides cambiar algo en el interior de tu objeto (como el motor del coche), no tienes que preocupar de que los usuarios o programadores se rompan la cabeza tratando de entender cómo funciona todo. Solo tendrán que seguir usando los métodos (botones) que ya conocen.
**En resumen:**
La ocultación significa esconder los detalles internos de un objeto y solo permitir que los demás interactúen con él de una manera controlada. Como con la caja secreta o el coche, solo tienes que saber cómo usarlo, no cómo funciona todo por dentro. 😄
## Polimorfismo! 😄

Imagina que tienes diferentes tipos de animales, como perros, gatos y pájaros. Todos son diferentes, pero tienen algo en común: pueden hacer el sonido que les corresponde.

Ahora, aquí viene lo interesante. Aunque cada uno de estos animales haga un sonido distinto, todos tienen una función llamada hacerSonido. Pero cada animal la va a hacer de manera diferente.

El perro dice: "¡Guau!"
El gato dice: "¡Miau!"
El pájaro dice: "¡Pío!"
Esto es polimorfismo en acción. Polimorfismo es la idea de que algo puede tomar diferentes formas, dependiendo del contexto. En programación, esto significa que puedes usar el mismo nombre para una función (o método) en diferentes clases, pero cada clase la implementa de manera diferente.

**¿Qué significa esto en programación?**
En programación orientada a objetos, el polimorfismo es cuando un método o función se comporta de manera diferente según el tipo de objeto que lo está usando.

**Ejemplo con código (o animales):**
Supongamos que tenemos una clase llamada Animal. Y de esta clase, podemos tener varias subclases como Perro, Gato y Pájaro. Todas estas subclases pueden tener un método llamado hacerSonido, pero cada uno lo hace de manera distinta.
```java
// Clase base: Animal
class Animal {
    // Método hacerSonido que será implementado por las subclases
    public void hacerSonido() {
        // Método vacío, porque será implementado en las subclases
    }
}

// Clase Perro que hereda de Animal
class Perro extends Animal {
    @Override
    public void hacerSonido() {
        System.out.println("¡Guau!");
    }
}

// Clase Gato que hereda de Animal
class Gato extends Animal {
    @Override
    public void hacerSonido() {
        System.out.println("¡Miau!");
    }
}

// Clase Pajaro que hereda de Animal
class Pajaro extends Animal {
    @Override
    public void hacerSonido() {
        System.out.println("¡Pío!");
    }
}

public class Main {
    public static void main(String[] args) {
        // Crear una lista de diferentes animales
        Animal[] animales = { new Perro(), new Gato(), new Pajaro() };
        
        // Usar el mismo método hacerSonido para todos los animales
        for (Animal animal : animales) {
            animal.hacerSonido();  // Aquí se aplica polimorfismo
        }
    }
}


```
Ahora, si tenemos una lista de diferentes animales, todos pueden usar el método hacerSonido, pero el sonido que hacen es diferente.
##### Explicación:
**Clase Animal:**
Tiene un método hacerSonido() que está vacío. Es solo una plantilla para las subclases.

**Subclases Perro, Gato, y Pájaro:**
Cada subclase sobrescribe el método hacerSonido() de la clase Animal y le da su propia implementación.
Polimorfismo:

En el método main(), tenemos un arreglo animales[] que contiene objetos de tipo Perro, Gato, y Pájaro. Todos son tratados como objetos tipo Animal.
Cuando llamamos al método hacerSonido() para cada animal, Java ejecuta el método adecuado según el tipo real del objeto (es decir, si es un perro, gato o pájaro). Esto es polimorfismo en acción.
Aunque todos los animales están usando el mismo método (hacerSonido), el resultado es distinto según el tipo de animal.

**¿Por qué es útil el polimorfismo?**
**Facilita el código:** No tienes que escribir un montón de código repetido. Usas el mismo nombre para el método, y el comportamiento cambia según el tipo de objeto.
**Flexibilidad:** Puedes agregar nuevos tipos de animales (o cualquier tipo de objeto) sin tener que cambiar el código que usa el método hacerSonido.
**Simplificación:** En lugar de tener que escribir diferentes nombres de métodos para cada tipo de animal, puedes llamarlos de manera común (en este caso, hacerSonido), y el sistema se encarga de llamar al método correcto.
**En resumen:**
El polimorfismo es como un superpoder que permite que diferentes objetos usen el mismo nombre de función, pero cada uno lo haga de manera distinta. Es una forma de hacer que tu código sea más flexible y más fácil de usar, ¡como si todos los animales pudieran hacer su propio sonido con solo presionar un botón! 😄

## Herencia
**¿Qué es la herencia en la programación orientada a objetos?**
La herencia es un concepto en programación que nos permite crear nuevas clases basadas en clases que ya existen. Es como si tuvieras una clase o tipo de objeto padre, y luego creas una nueva clase o tipo de objeto hijo que hereda las características y comportamientos del padre. El hijo puede tener cosas propias, pero también hereda muchas de las cosas que ya tiene el padre.

**¿Cómo funciona la herencia?**
Imagina que tienes una clase de animales y dentro de esa clase defines comportamientos como comer o dormir. Luego, puedes crear nuevas clases como Perro o Gato, y esas clases heredan las acciones comunes de la clase de animales, como comer o dormir. Pero también pueden tener sus propios comportamientos o características, como ladrar o maullar.

**Ejemplo en la vida real:**
Imagina una familia. Los hijos pueden heredar muchas características de los padres: por ejemplo, el color de ojos, el color de cabello, la forma de caminar, etc. Pero también los hijos pueden tener sus propias características que no necesariamente tienen los padres.

El padre tiene ojos azules.
El hijo hereda los ojos azules de su padre, pero puede ser muy bueno en el fútbol, mientras que el padre no lo es.
De forma similar, en programación, la herencia permite a los objetos hijos heredar cosas de los objetos padres.

**Ejemplo de herencia en código (en Java):**
Imagina que estamos creando un sistema para diferentes tipos de vehículos. Todos los vehículos tienen algunas cosas en común, como arrancar y detenerse. Pero también tienen características especiales, por ejemplo, un automóvil tiene un número de ruedas y una moto tiene un manillar.

Primero, creamos una clase base llamada Vehiculo con características comunes:
```java
// Clase base o clase padre
class Vehiculo {
    // Atributos comunes a todos los vehículos
    String color;
    int ruedas;

    // Método común
    public void arrancar() {
        System.out.println("El vehículo está arrancando.");
    }

    public void detenerse() {
        System.out.println("El vehículo se ha detenido.");
    }
}

```

Luego, podemos crear clases hijas que heredan de la clase Vehiculo. Estas clases hijas pueden agregar sus propios comportamientos (como características específicas de un automóvil o una moto), pero heredan las acciones generales como arrancar() y detenerse():
```java
// Clase hija: Automóvil
class Automovil extends Vehiculo {
    // Características propias del Automóvil
    int puertas;

    // Método propio de la clase Automovil
    public void abrirPuertas() {
        System.out.println("Abriendo las puertas del automóvil.");
    }
}

// Clase hija: Moto
class Moto extends Vehiculo {
    // Características propias de la Moto
    boolean tieneManillar;

    // Método propio de la clase Moto
    public void hacerCaballito() {
        System.out.println("¡Estoy haciendo un caballito!");
    }
}

```
**¿Cómo se usa la herencia?**
Ahora podemos crear objetos de la clase Automovil y Moto, y estos objetos tendrán tanto las características que definimos en Vehiculo (como arrancar y detenerse) como las características propias de cada tipo de vehículo.
```java
public class Main {
    public static void main(String[] args) {
        // Creamos un objeto de la clase Automovil
        Automovil miAuto = new Automovil();
        miAuto.arrancar(); // Heredado de Vehiculo
        miAuto.abrirPuertas(); // Propio de Automovil

        // Creamos un objeto de la clase Moto
        Moto miMoto = new Moto();
        miMoto.arrancar(); // Heredado de Vehiculo
        miMoto.hacerCaballito(); // Propio de Moto
    }
}

```

**¿Qué pasa con la herencia aquí?**
Heredamos métodos comunes: Como puedes ver, tanto Automovil como Moto heredan los métodos arrancar() y detenerse() de la clase Vehiculo, sin necesidad de reescribirlos.

Cada clase hija puede tener sus propios métodos: Como abrirPuertas() para el Automovil o hacerCaballito() para la Moto. Estas son características específicas de cada clase hija.

**¿Por qué es útil la herencia?**
Reutilización de código: Si varias clases tienen comportamientos o características similares, puedes reutilizar el código en lugar de tener que escribirlo una y otra vez. La clase hija solo tiene que heredar lo que ya está en la clase padre.
**Simplificación:** Al tener una clase base con los comportamientos comunes, puedes hacer que el código sea más organizado y más fácil de mantener.
**Extensibilidad:** Si en el futuro quieres agregar más tipos de vehículos, como bicicletas o camiones, puedes heredar de Vehiculo sin tener que reescribir todos los comportamientos comunes.

**Resumen de la herencia:**
Herencia es un concepto que permite que una clase herede comportamientos y características de otra clase.
Te permite reutilizar código y crear nuevas clases sin tener que empezar desde cero.
Una clase hija puede agregar sus propios comportamientos o modificar los heredados de la clase padre.

## Cohesión
La cohesión en programación es como asegurarte de que todas las piezas de tu casa (o programa) trabajen bien juntas.

Cohesión en programación y POO:
En el contexto de la Programación Orientada a Objetos (POO), cohesión significa que todas las partes de una clase deberían estar relacionadas entre sí y enfocarse en una sola tarea o responsabilidad. Es como si estuvieras construyendo una habitación que solo tiene una función, por ejemplo, una cocina. Si en esa habitación empiezas a poner cosas que no tienen nada que ver con cocinar, como una cama o una televisión, ¡no tendría sentido!

Imagina una clase como una habitación:
Una clase es como una habitación de tu casa, y dentro de esa clase, tienes métodos y atributos (cosas que hacen o tienen los objetos).

Ejemplo de una clase con baja cohesión:
Supongamos que tenemos una clase llamada Casa, pero dentro de ella mezclamos muchas cosas que no están relacionadas:
```JAVA
class Casa {
    String color;
    int habitaciones;
    
    // Método para cocinar
    void cocinar() {
        System.out.println("Cocinando...");
    }
    
    // Método para pintar la casa
    void pintarCasa() {
        System.out.println("Pintando la casa...");
    }
    
    // Método para dormir
    void dormir() {
        System.out.println("Durmiendo...");
    }
}

```
En este ejemplo, la clase Casa tiene muchas responsabilidades que no están relacionadas entre sí. Por ejemplo, cocinar, pintar la casa y dormir son cosas completamente diferentes, ¿verdad? Esto hace que la clase tenga baja cohesión.

Ahora, una clase con alta cohesión:
En una clase con alta cohesión, cada clase tiene una responsabilidad clara y se enfoca en una sola tarea. Es como tener una habitación para cada cosa: una para dormir, otra para cocinar, otra para estudiar, etc. Así que podemos crear clases separadas para cada tarea.

Ejemplo de alta cohesión:
```java
class Cocina {
    void cocinar() {
        System.out.println("Cocinando...");
    }
}

class Dormitorio {
    void dormir() {
        System.out.println("Durmiendo...");
    }
}

class Casa {
    String color;
    int habitaciones;
}

```
Ahora, cada clase está enfocada en una tarea específica: la clase Cocina solo se encarga de cocinar, la clase Dormitorio solo se encarga de dormir, y la clase Casa solo guarda la información sobre la casa. ¡Cada clase tiene un propósito claro! Esto hace que las clases sean cohesivas.

¿Por qué es importante la cohesión en POO?
Facilita el mantenimiento: Si alguna cosa se rompe o cambia (como si quieres cambiar cómo cocinas), solo tienes que ir a la clase Cocina, no a la clase Casa entera.

Hace que tu código sea más limpio: Es más fácil entender qué hace cada parte del programa cuando todo está organizado y enfocado en una sola tarea.

Menos errores: Al dividir las responsabilidades, es más fácil encontrar problemas, porque no hay muchas cosas mezcladas en una sola clase.

Resumen:
La cohesión en POO es como organizar tu casa de manera que cada habitación tenga una función específica y todo esté en su lugar. Si cada clase se enfoca en hacer una sola cosa bien, el programa será más fácil de entender y de mantener.