## What's YAML?
- Its a 
##### YAML
**Key Value Pair**
```java
Fruit: Apple
Vegetable: Carrot
Liquid: Water
Meat: Chicken
```
- Fruit = Key
- Apple = Value
- :(a space)= colon + space
We must have a space followed by a colon.

**Array/Lists**
```java
Fruits: 
-	Orange
-	Apple
-	Banana

Vegetables:
-	Carrot
-	Cauliflower
-	Tomato
```
The dash indicates that it's an element of an array

**Dictionary/Map**
It's a set of properties grouped together under an item
```java
Banana:
	Calories: 105
	Fat: 0.4g
	Carbs: 27g

Grapes:
	Calories: 62
	Fat: 0.3g
	Carbs: 16g
```
##### **When to use a dictionary or a list?**
**Dictionary vs List vs List of Dictionaries**
XML, JSON, YAML are used to represent data.

 **Dictionary**
- To store different information or properties of a single object.
```java
//a car a simple object
Color: Blue
Model: Corvette
Transmission: Manual
Price: $23,000
```

**Dictionary In Dictionary**
-  We could use dictionary within another dictionary
```java
//a car a simple object
Color: Blue
Model: 
	Name: Corvette
	Model: 1995
Transmission: Manual
Price: $23,000
```

**List/Array**
- We would like to store the name of 6 cars, name and color.
- It is multiple items of the *same* type of object
```java
//List Of String
- Blue Corvette
- Grey Corvette
- Red Corvette
- Green Corvette
- Purple Corvette
- Black Corvette
```

**List/Array Of Dictionary**
- We can represent all info about multiple cars in a single yml file using a list of dictionaries.
```java
- Color: Blue
  Model: 
	  Name: Corvette
	  Model: 1995
  Transmission: Automatic
  Price: $23,000
  
- Color: Grey
  Model: 
	  Name: Corvette
	  Model: 1995
  Transmission: Manual
  Price: $22,000
  
- Color: Red
  Model: 
	  Name: Corvette
	  Model: 1995
  Transmission: Manual
  Price: $21,000
  
- Color: Green
  Model: 
	  Name: Corvette
	  Model: 1995
  Transmission: Automatic
  Price: $23,000
  
- Color: Blue
  Model: 
	  Name: Corvette
	  Model: 1995
  Transmission: Manual
  Price: $25,000

- Color: Black
  Model: 
	  Name: Corvette
	  Model: 1995
  Transmission: Automatic
  Price: $22,000  
```
##### YAML - NOTES
***Dictionary/Map*** Is an ***unordered*** collection
***List/Array*** Are ***ordered*** collection
***Hash #*** Is for Comments.

## LAB
***Which of the following is used to separate the key and value in YAML?***
- colon
***How many array keys are there in the following yaml snippet?***
```java
Fruits:
  - Orange
  - Apple
  - Banana
Vegetables:
  - Carrot
  - CauliFlower
  - Tomato
```
- This yaml snippet has 2 array keys, Fruits and Vegetables.
***Which of the following statements is true?
	A. Dictionary is an unordered collection whereas list is an ordered collection
	B. Dictionary is an ordered collection whereas list is an unordered collection.
	C. Dictionary and list, both are an ordered collection.
	D. Dictionary and list, both are an unordered collection.***
- A

***we would like to add additional details for each item, such as color, weight etc. Modify the remaining items to match the below data.***
orange
color	weight
orange	90g

mango
color	weight
yellow	150g***
```java
- name: apple
  color: red
  weight: 100g
- name: orange
  color: orange
  weight: 90g
- name: mango
  color: yellow
  weight: 150g
```
***We would like to record information about multiple employees. Convert the dictionary named employee to an array named employees.***

```java
employees:
  - name: john
    gender: male
    age: 24
```

***Now add the pay information. Remember, while address is a dictionary, payslips is an array of month and amount.***

payslips

month	amount
june	1400
july	2400
august	3400

```
employee:
  name: john
  gender: male
  age: 24
  address:
    city: edison
    state: new jersey
    country: united states
  payslips:
    - month: june
      amount: 1400
    - month: july
      amount: 2400
    - month: august
      amount: 3400
```