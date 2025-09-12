#### SLICING
Slicing (rebanado): El slicing en Python sigue la siguiente sintaxis:

> [!TIP] list[start:end]
> start: Es el índice desde donde se empieza a cortar (inclusivo).
> end: Es el índice hasta donde se corta (pero no se incluye).
> Si omites el end como en my_list[2:], Python cortará desde el índice 2 hasta el final de la lista.
> **El índice -1** en Python se usa para acceder al último elemento de la lista.
Los índices negativos en Python permiten acceder a los elementos desde el final de la lista:
-1 es el último elemento.
-2 es el penúltimo.
-3 es el antepenúltimo, y así sucesivamente.


> [!WARNING] DEL + SLICING
> It will delete the original list!


> [!tip] list[start:end:step]
> start: El índice de inicio (inclusive), donde comienza la rebanada.
end: El índice de fin (exclusive), donde termina la rebanada.
step: El paso o incremento entre los índices. Es lo que determina cuántos elementos saltar al recorrer la lista.
my_list = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
 **Usando slicing con step (paso 3)**
print(my_list[::3])
[0, 3, 6, 9]
 **Tomando los elementos desde el índice 1 hasta el índice 7, con un paso de 2**
print(my_list[1:7:2])     
  [1, 3, 5]
 **Invirtiendo la lista con un paso negativo**
print(my_list[::-1])
Esto crea una nueva lista que es la inversión de la original.
[9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
my_list = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(my_list[:])  
Esto dará: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]


> [!summary] Resumen
> [::]: Toma toda la lista.
[::step]: Toma la lista con un paso específico.
[start:end:step]: Define un rango con un paso específico entre los índices.
list1 = [0, 3, 4, 1, 2]  # **Lista original**
list2 = list1[:]  # **Copia de la lista original**

#### NESTED LISTS - MATRIX 2D
```PYTHON
a = []
for i in range(5):
    a.append([])
    for j in range(5):
        a[i].append(j)

print(a[2][3])
```
**1. Inicialización de a:**
- a = \[] crea una lista vacía, que luego se llenará con otras listas.

**2. Primer bucle for i in range(5):**
- Este bucle ejecuta 5 veces (cuando i va de 0 a 4).
- En cada iteración, añades una lista vacía (a.append(\[])) a la lista a.
- Al final de este bucle, la lista a se verá así:
```PYTHON
a = [ [], [], [], [], [] ]
```
**3. Segundo bucle for j in range(5):**
- Dentro del primer bucle, hay un segundo bucle que también itera 5 veces (cuando j va de 0 a 4).
- En cada iteración del segundo bucle, se añade el valor j a la lista a[i]. Es decir, en cada fila de a, se agregan los valores del 0 al 4.

Después de ejecutar ambos bucles, la lista a se ve así:
```PYTHON
a = [
    [0, 1, 2, 3, 4],
    [0, 1, 2, 3, 4],
    [0, 1, 2, 3, 4],
    [0, 1, 2, 3, 4],
    [0, 1, 2, 3, 4]
]
// Resultado final: 3
```
**4. Acceso a a[2][3]:**
- a\[2] accede a la tercera lista en la lista a (recordando que los índices empiezan desde 0), que es [0, 1, 2, 3, 4].
- a\[2]\[3] accede al cuarto valor de esa lista (índice 3), que es 3.


> [!TIP] COMPRENSIÓN POR LISTAS
> [[expresión for item in iterable] for item in iterable2]
> **matrix = [[j for j in range(5)] for i in range(5)]**
> Lo que significa que, por cada item en iterable2, se genera una sublista (que es la comprensión de lista interna), y luego todas esas sublistas se almacenan en la lista principal.
> 

**for i in range(5): Este es el bucle externo.** Está creando 5 sublistas, ya que range(5) produce los valores 0, 1, 2, 3, 4. Es decir, va a crear 5 filas en la matriz.

**\[j for j in range(5)]:** Esto es la comprensión interna. Dentro de cada iteración del bucle externo, se genera una lista de los números 0, 1, 2, 3, 4 porque range(5) produce esos valores. Así, para cada valor de i, se crea una sublista \[0, 1, 2, 3, 4].
La lista matrix será una matriz de 5x5 (5 filas y 5 columnas):
```PYTHON

matrix = [
    [0, 1, 2, 3, 4],
    [0, 1, 2, 3, 4],
    [0, 1, 2, 3, 4],
    [0, 1, 2, 3, 4],
    [0, 1, 2, 3, 4]
]
```

> [!SUMMARY] RESUMEN
> **Parte externa:**
El bucle for i in range(5) itera 5 veces, generando 5 sublistas.
**Parte interna:**
El bucle for j in range(5) crea una sublista con los números de 0 a 4 para cada iteración del bucle externo.

```PYTHON
countries = [['Egypt', 'USA', 'India'], ['Dubai', 'America', 'Spain'], ['London', 'England', 'France']]
countries2  = [country for sublist in countries for country in sublist if len(country) < 4]
print(countries2)
// 'USA'
```

```PYTHON
matrix = [[0, 1, 2], [0, 1, 2], [0, 1, 2]]

matrix2 = []

for submatrix in matrix:
  for val in submatrix:
    matrix2.append(val)

print(matrix2[0])
//val = 0,1,2, 0,1,2, 0,1,2 //lista plana
// 0
```

#### NESTED LISTS - 3D
```PYTHON
school_array = [
    # Primer grado, en el piso 0
    [
        ["Alice", "Bob", "Charlie"],  # Grupo A del primer grado
        ["David", "Eva", "Frank"]     # Grupo B del primer grado
    ],
    # Segundo grado, en el piso 1
    [
        ["Grace", "Hannah", "Ivy"],   # Grupo A del segundo grado
        ["Jack", "Liam", "Mia"]       # Grupo B del segundo grado
    ],
    # Tercer grado, en el piso 2
    [
        ["Noah", "Olivia", "Paul"],   # Grupo A del tercer grado
        ["Quinn", "Rachel", "Sam"]    # Grupo B del tercer grado
    ]
]
```

##### Imprimir todos los nombres de los estudiantes
```PYTHON
for grade_level, grade in enumerate(school_array, start=1):
    print(f"Grado {grade_level}:")
    for group_idx, group in enumerate(grade, start=1):
        print(f"  Grupo {group_idx}: {', '.join(group)}")
    print()
______________________________________________
Grado 1:
  Grupo 1: Alice, Bob, Charlie
  Grupo 2: David, Eva, Frank

Grado 2:
  Grupo 1: Grace, Hannah, Ivy
  Grupo 2: Jack, Liam, Mia

Grado 3:
  Grupo 1: Noah, Olivia, Paul
  Grupo 2: Quinn, Rachel, Sam
```

> [!summary] How to read 3D lists
> 1.  We focus as a school building, choose the floor
> 2. Choose the row
> 3. Choose the column
> 4. LIAM [1][1][1]
```PYTHON
Colors= [ 
		[['Blue','Green','White','Black']],
		[['Green','Blue','White','Yellow']] , 
		[['White','Blue','Red','Green']] 
]
# RED [2][0][2]
```

```PYTHON
matrix = [[[k for k in range(3)] for j in range(3)] for i in range(3)]
print(matrix[2][1][1])  
# Esto imprimirá el número en la segunda columna de la tercera fila
print(matrix[2][1]) 
#te da la sublista completa de una fila/row, que contiene los números.
```

```python
Colors = [ ['Red', 'Green', 'White', 'Black'], 
           ['Green', 'Blue', 'White', 'Yellow'], 
           ['White', 'Blue', 'Green', 'Red'] ]

# To get the third element in the second row:
third_element_in_second_row = Colors[1][2]
print(third_element_in_second_row)
```

##### PASAR DE 3D A 2D
**Matriz 3D (original):**
```PYTHON
matrix = [
    [ [0, 1, 2], [0, 1, 2], [0, 1, 2] ],
    [ [0, 1, 2], [0, 1, 2], [0, 1, 2] ],
    [ [0, 1, 2], [0, 1, 2], [0, 1, 2] ]
]
```
- Es una lista de listas de listas (3D).
- Tiene 3 submatrices.
- Cada submatriz tiene 3 listas.
- Cada lista tiene 3 elementos.

**Matriz 2D (aplanada):**
```python
matrix2 = [
    [0, 1, 2],  # sublista de la **primera** submatriz
    [0, 1, 2],  # sublista de la primera submatriz
    [0, 1, 2],  # sublista de la primera submatriz
    [0, 1, 2],  # sublista de la **segunda** submatriz
    [0, 1, 2],  # sublista de la segunda submatriz
    [0, 1, 2],  # sublista de la segunda submatriz
    [0, 1, 2],  # sublista de la **tercera** submatriz
    [0, 1, 2],  # sublista de la tercera submatriz
    [0, 1, 2]   # sublista de la tercera submatriz
]
```
- Es una lista de listas 2D.
- Tiene todas las sublistas de todas las submatrices concatenadas.
- Ahora tienes 9 sublistas en lugar de 3 submatrices.

**Al acceder a matrix2\[2]\[0], estás accediendo a la tercera sublista de matrix2:**
`matrix2[2] = [0, 1, 2]`
**Luego, matrix2\[2]\[0] accede al primer elemento de esa sublista, que es 0.**