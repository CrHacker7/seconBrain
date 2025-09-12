#### FUNCTIONS
**What is the output of the following snippet:**

```PYTHON
def my_function(*students):
  print("The tallest student is " + students[2])
my_function("James", "Ella", "Jackson")
#The tallest student is Jackson
#If the number of arguments is unknown, we can add a * before the parameter name.
```

Q:**The function can have only one parameter. If any data (parameters) are passed, they are passed explicitly.**
False

**What is the error in the following snippet code:**
```python
def multi_func():
  result = int(input()) * 5
  return result     

print(result)
#O erro ocorreu porque você tentou usar uma variável fora do seu escopo.
#A solução é chamar a função e usar o return corretamente.
print(multi_func())
```

**What is the output of the following snippet:**
```python
def my_function(*students):
  print("The tallest student is " + students[2])
my_function("James", "Ella", "Jackson")
#the tallest student is Jackson
```
**Method is called by its name, but it is associated with an object.**
True

**Define a function that gets the user input and multiply it by a number we passed to the function.**
```python
Sample input:  6
Sample output: 

print( multi_num(5) )
30
#def multi_num(num): return int(input()) * num
```

**What will be the result of calling the print_info function with the arguments 'john' and 19?**

```python
def print_info(name, age=18):
    print(name, age)

print_info('john', 19)
#john 19
```

**Choose the correct answer which defines a function to get numeric input from the user:**
`def input_num(): return int(input())`


> [!NOTE]- Parâmetro
> É o nome da variável definido na declaração da função.
> Ele representa um valor que a função espera receber quando for chamada.
> Só existe dentro da definição da função.

> [!NOTE]-  Argumento 
> É o valor real que você passa para a função quando a chama.
> É o dado que será associado ao parâmetro da função.

The output of the following snippet of the code will be:

```python
nums= [7,4,1]
def change_third_item(list):
	list[2] = 5

change_third_item(nums)
print(nums)
# [7,4,5]
```

**The output of the following snippet of the code will be:**
```python
a = 0
def add_three(a):
	return a+3

result = add_three(3)
print(result)
```

**Define a function that gets the user input and add to it a number we passed to the function.**

```python
Sample input:  6
Sample output: 
print( add_num(5) )
11
___________
def add_num(num):
   return int(input()) + num
```

```python
Function :  def add_func(num1,num2):
            	  	return num1 + num2 
		
Sample input: print ( add_func(5 , 5) )
#10
```

```python
def my_function(*friends):
  print("The tallest student is " + friends[0])

my_function("john", "Ella", "mark")
#The tallest student is john
```

```python
def is_true(a): 
  return bool(a) #it changes to boolean

result = is_true(6<3) 
print("The result is", result)
#the result is False
```

**The output of the following snippet of the code will be:**

```python
def square(i):
    j = i * i
    return j

print(square(3))
#9
```

```python
def my_function(x):
  return 10 / x

print(my_function(2))
#5.0 (division always returns decimals)
```
#### LIST AS ARGUMENTS
```PYTHON
def mean_func(list1):
    return sum(list1) / len(list1)

print(mean_func([5, 6, 7, 8]))
#6.5
```

```PYTHON
def get_even_func(numbers):
    even_numbers = [num for num in numbers if not num % 2]
    return even_numbers

get_even_func([1, 2, 3, 4, 5, 6])
```

> [!hint] Inverse logic
> "Si NO sobra nada al dividir entre 2 (es decir, si es par), entonces sí se incluye."
>- num % 2 da 0 si num es par → False en contexto booleano
>- num % 2 da 1 si num es impar → True en contexto booleano
>**Al aplicar el not:**
>- Si num % 2 es 0 (False), not 0 es True → entra al if
>- Si num % 2 es 1 (True), not 1 es False → no entra al if

> [!warning] tip
> El truco está en cómo se evalúa num % 2:
> Cuando un número es divisible entre 2 (par), num % 2 es 0, y en Python eso es considerado False en un condicional.
> 
> Cuando un número no es divisible entre 2 (impar), num % 2 es 1, que es True.
> 
num % 2 → “¿cuánto sobra cuando divides num por 2?”
not num % 2 → “*** no sobra*** nada cuando divides num por 2” (o sea, es divisible)

```python
def get_odd_func(numbers):
    odd_numbers = [num for num in numbers if num % 2]
    return odd_numbers

print(get_odd_func([7, 4, 5, 6, 9, 8, 12]))
```
> [!tip]
> si sobra al dividirlo entre 2 entonces es impar por lo tanto se añade

#### SCOPES
```python
def my_function():
  x = 20 # HERE IS AS A GLOBAL VAR BUT INSIDE THE FUNCTION
  def my_inner_function():
    print(x)
  my_inner_function()
my_function()
# THE LOCAL VARIABLE CAN BE ACCESSED FROM A FUNCTION WITHIN THE FUNCTION
```

```PYTHON
def my_function():
  def my_inner_function():
    x = 20
  print(x)
  my_inner_function()

my_function()
```

```PYTHON
def my_function(arg1, *argv): 
    print ("First argument:", arg1) 
    for arg in argv: 
        print("Next argument:", arg) 

my_function('Welcome', 'to', 'Python!')
#First argument: Welcome
#Next argument: to
#Next argument: Python!
```

```python
def my_function(*argv):  
    for arg in argv:  # para imprimir cada elem por separado!
        print(arg) 

my_function('Hello', 'World!')
#Hello
#World!

#The syntax is to use the symbol * to tale in a variable number of arguments
```

**¿Qué es una tupla?**
Una tupla es una estructura de datos similar a una lista, pero inmutable (no se puede modificar).
- Añadir una coma para que py sepa que es una tuple sino se confunde con una var
- Puede recibir distintos tipos de datos.
- No se puede .append() porque sería modificar.
- Puede tener paréntesis o no.
En Python, las tuplas se muestran con paréntesis () cuando las imprimes.
```python
mi_tupla = ('Hello', 'World!')
print(mi_tupla)  # Salida: ('Hello', 'World!')

x = ()# empty tuple
```

> [!TIP] TUPLE
> A TUPLE is a collection of items that are ordered, unchangeable, and allow duplicate values.

**Access value 30 from the following tuple:**
```a = (10, [20, 30], 40, 50)
a[1][1]
```

**A dictionary is a collection that is ORDERED, CHANGEABLE and DOES NOT ALLOW DUPLICATES**

popitem() Elimina el último de la lista del dictionary.
**Which of the following method is used to delete a brand's key and its value from the following dictionary:**
```
testdict = {'brand': 'oppo', 'ram': '3', 'Os': 'Android', 'year': 2020}
del test['brand']
```

```
testdict = {
  "brand": "Samsung",
  "ram": "3",
  "Os": "Android",
  "year": 2020
}

print(testdict.items())
#dict_items([
            ('brand' , 'Samsung'),
            ('ram', '3'),
            ('Os', 'Android'),
            ('year', '2020')
            ])
```