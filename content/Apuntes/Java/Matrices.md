- el primer for recorre las filas
- el segundo for anidado recorre sus columnas
- el total de la columna se accede a través de la fila. 
	- `for(int j = 0; j < names[i].length; j++`
También se puede usar con for each
```java
for(String[] fila: names){
	for(String column: fila){
		System.out.println("names" + column);	
	}
}
```


