Para fusionar tu rama local main con tu rama remota master y dejar solo una rama, sigue estos pasos. Este proceso asegura que tus cambios en la rama main se integren en master y que puedas eliminar la rama que ya no necesites. 

1. Asegúrate de estar en la rama correcta (main):
Primero, asegúrate de que estás en la rama local main. Si no estás en main, cámbiate a ella con:


git checkout main
2. Trae la última versión de la rama remota master:
Ahora, necesitas asegurarte de que tienes la última versión de la rama remota master antes de fusionar.


git fetch origin
Esto descargará los cambios de las ramas remotas, incluyendo master.

3. Fusiona tu rama local main con master:
A continuación, fusiona los cambios de main en master (la rama remota). Primero, asegúrate de estar en la rama main, y luego ejecuta el siguiente comando para fusionarla con la rama remota master.


git merge origin/master
Este comando fusionará los cambios de master desde el repositorio remoto en tu rama local main.

Si no hay conflictos, el merge se realizará automáticamente.
Si hay conflictos, Git te pedirá que los resuelvas. Una vez resueltos los conflictos, añade los archivos modificados con git add y luego haz un commit con git commit.
4. Subir tus cambios fusionados a master:
Una vez que hayas fusionado los cambios, sube la rama main al repositorio remoto. Sin embargo, esto no sube directamente a master. Debes fusionar main en master en el repositorio remoto.

Para hacerlo, primero asegúrate de que estás en main y luego realiza el push:


git push origin main
5. Fusiona los cambios en el repositorio remoto (master):
Ahora que main ha sido empujada al repositorio remoto, debes fusionar la rama main con la rama remota master. Puedes hacer esto directamente desde GitHub (u otro servicio de repositorios), creando un pull request o directamente desde la terminal (si tienes permisos).

Si deseas hacer el merge desde la terminal, primero cambia a la rama master:


git checkout master
Luego, fusiona main en master:


git merge main
Finalmente, sube los cambios de la rama master al repositorio remoto:


git push origin master
6. Elimina la rama remota main y local main:
Si todo ha ido bien y ya tienes todos tus cambios fusionados en master, puedes eliminar la rama local main y la remota main.

Eliminar la rama remota main:

git push origin --delete main
Eliminar la rama local main:

git branch -d main
Si por alguna razón la rama main no se ha fusionado completamente, puedes forzar la eliminación con:


git branch -D main
7. Actualiza tus referencias locales:
Para asegurarte de que las ramas eliminadas ya no aparecen en tus referencias locales, ejecuta:


git fetch --prune
Resumen de pasos:
Asegúrate de estar en main: git checkout main.
Actualiza tu repositorio local con los cambios remotos: git fetch origin.
Fusiona main con master: git merge origin/master.
Sube los cambios a main (si es necesario): git push origin main.
Fusiona main en master en el repositorio remoto (desde terminal o mediante Pull Request).
Elimina las ramas locales y remotas main: git push origin --delete main y git branch -D main.
Actualiza las referencias locales: git fetch --prune.
Con estos pasos, habrás fusionado tu rama main con master y eliminado la rama no deseada. 
