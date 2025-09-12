#### Copiar directorio de linux a windows** 
`cp -r /home/carly/DevOpsVault/Doc-DevOps/DevOps-Ultimate /mnt/c/Users/crojas/Desktop/myVault/`

#### Crear carpetas y subcarpetas
1. Dentro de la carpeta que queremos crear los subcarpetas
```bash
\Documents\Java Master> for ($i = 1; $i -le 106; $i++) { mkdir ("Sección " + $i.ToString("D3")) }
```
**for ($i = 1; $i -le 106; $i++)** Este es el bucle for en PowerShell, que comienza con $i = 1, y sigue hasta que $i sea menor o igual a 106.

**$i -le 106** es la condición que evalúa si el valor de $i ha llegado a 106.
**$i++** es lo que incrementa el valor de $i en cada iteración.
mkdir ("Sección " + $i.ToString("D3")):

**mkdir** es el comando para crear una carpeta.
**"Sección " + $i.ToString("D3")** convierte el valor de $i a una cadena de texto con tres dígitos, como "Sección 001", "Sección 002", ..., "Sección 106".
La función **.ToString("D3")** te asegura que los números estén formateados con tres dígitos

``` bash
icacls "Sección 001" /grant "TU_USUARIO":F

```