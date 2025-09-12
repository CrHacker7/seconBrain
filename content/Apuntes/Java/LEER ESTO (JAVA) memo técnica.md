Las expresiones lambda en Java permiten pasar funciones como argumentos o usarlas de manera más concisa en lugar de escribir clases anónimas. Para muchas personas, esto puede ser un concepto bastante difícil de entender, ya que se aleja del estilo tradicional de programación orientada a objetos.

Concepto Abstracto: Expresiones Lambda y Interfaces Funcionales
¿Qué son?
Expresión Lambda: Es una forma concisa de escribir una función anónima (sin tener que crear una clase que implemente una interfaz).
Interface Funcional: Una interfaz que tiene exactamente un método abstracto. Las expresiones lambda pueden ser usadas para implementar estas interfaces de manera más breve.

```
// Interface funcional
@FunctionalInterface
interface Operacion {
    int aplicar(int a, int b);  // un método abstracto
}

// Uso de expresión lambda
Operacion suma = (a, b) -> a + b;

```



1. El Método de Elaboración: Asociar con algo que ya sabes
Imagina que tienes una máquina que puede realizar operaciones matemáticas. Esta máquina tiene un "botón" para cada tipo de operación, como suma, resta, multiplicación, etc.

En lugar de tener que construir una máquina completa para cada operación (como lo harías con clases tradicionales), una expresión lambda es como un programador que te permite agregar nuevos botones rápidamente a la máquina sin tener que crear toda una clase para cada operación.

La interfaz funcional es el diseño del botón, que establece qué operación debe hacer (por ejemplo, sumar, restar).
La expresión lambda es como configurar el comportamiento de ese botón, es decir, qué hace exactamente el botón cuando lo presionas. Por ejemplo, "cuando presiono este botón, haz la suma de dos números".
1. Asociación Visual: Crear una imagen mental
Imagina que tienes una máquina de operaciones matemáticas. Cada botón de la máquina es un método de una interfaz funcional. La máquina no sabe qué operación hace hasta que le dices con una expresión lambda qué debe hacer cuando presionas ese botón.

Visualiza un panel de botones en una máquina, y cada botón tiene una función específica (por ejemplo, uno suma, otro multiplica, otro resta).
La interfaz funcional es como el diseño de los botones: cada botón tiene un comportamiento común (realizar una operación con dos números), pero lo que hace cada botón depende de cómo lo configures.
Cuando presionas un botón, la expresión lambda define lo que hace ese botón (por ejemplo, a + b para un botón de suma).
Resumen visual del concepto:
Interfaz Funcional = Botón de la máquina que tiene una acción común (como aceptar dos números).
Expresión Lambda = Configuración del comportamiento del botón. ¿Qué hace ese botón? ¿Suma? ¿Resta? ¿Multiplica?
Máquina de Operaciones = El sistema que tiene estos botones para realizar operaciones matemáticas.
Visualización paso a paso:
Máquina de Operaciones (el sistema Java)
Botones (las interfaces funcionales): Cada botón tiene un método, como aplicar(int a, int b).
Expresión Lambda: La forma en que configuras el botón para que haga una operación específica. Por ejemplo, el botón de suma hará la operación (a, b) -> a + b.
Ejemplo visual:
Imagina una máquina con 3 botones: uno de suma, otro de resta y otro de multiplicación. Cada botón tiene la misma estructura (la interfaz funcional), pero cada uno realiza una operación diferente dependiendo de cómo lo configures con expresiones lambda:

Botón de suma: (a, b) -> a + b
Botón de resta: (a, b) -> a - b
Botón de multiplicación: (a, b) -> a * b
Recuerda:
Interfaz Funcional = "Botón" (especifica el comportamiento común).
Expresión Lambda = "Configuración del botón" (qué hace el botón).
