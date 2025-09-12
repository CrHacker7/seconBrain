Para fusionar tu rama local main con tu rama remota master y dejar solo una rama, sigue estos pasos. Este proceso asegura que tus cambios en la rama main se integren en master y que puedas eliminar la rama que ya no necesites. Aquí te explico cómo hacerlo de manera segura:

1. Asegúrate de estar en la rama correcta (main):
`git checkout main`
2. Trae la última versión de la rama remota master:
`git fetch origin`
Esto descargará los cambios de las ramas remotas, incluyendo master.
3. Fusiona tu rama local main con master:
A continuación, fusiona los cambios de main en master (la rama remota). Primero, asegúrate de estar en la rama main, y luego ejecuta el siguiente comando para fusionarla con la rama remota master.

`git merge origin/master`
Este comando fusionará los cambios de master desde el repositorio remoto en tu rama local main.

> HISTORIAS NO RELACIONADAS

### 1.  Forzar el Merge de las dos ramas sin perder cambios
Puedes hacer un merge no relacionado, es decir, ignorando el historial de ambas ramas y fusionando los cambios manualmente. Para hacerlo, debes usar la opción --allow-unrelated-histories al ejecutar el merge.

Estando en la rama main, haz el merge con master usando la opción --allow-unrelated-histories:
`git merge origin/master --allow-unrelated-histories`
Resuelve los conflictos (si los hay) de la misma forma en que lo harías con cualquier merge:

Abre los archivos conflictivos.
Haz las modificaciones necesarias.
Añade los cambios: `git add <archivo>.`
Haz un commit para completar la fusión:
`git commit`
Sube los cambios a tu repositorio remoto:
`git push origin main`
Ahora puedes seguir los pasos mencionados anteriormente para fusionar main con master en el repositorio remoto.

Si no hay conflictos, el merge se realizará automáticamente.
Si hay conflictos, Git te pedirá que los resuelvas. Una vez resueltos los conflictos, añade los archivos modificados con git add y luego haz un commit con git commit.
4. Subir tus cambios fusionados a master:
Una vez que hayas fusionado los cambios, sube la rama main al repositorio remoto. Sin embargo, esto no sube directamente a master. Debes fusionar main en master en el repositorio remoto.

Para hacerlo, primero asegúrate de que estás en main y luego realiza el push:
`git push origin main`
5. Fusiona los cambios en el repositorio remoto (master):
Ahora que main ha sido empujada al repositorio remoto, debes fusionar la rama main con la rama remota master. Puedes hacer esto directamente desde GitHub (u otro servicio de repositorios), creando un pull request o directamente desde la terminal (si tienes permisos).

Si deseas hacer el merge desde la terminal, primero cambia a la rama master:

`git checkout master`
Luego, fusiona main en master:
`git merge main`
Finalmente, sube los cambios de la rama master al repositorio remoto:
`git push origin master`
6. Elimina la rama remota main y local main:
Si todo ha ido bien y ya tienes todos tus cambios fusionados en master, puedes eliminar la rama local main y la remota main.

Eliminar la rama remota main:

`git push origin --delete main`
Eliminar la rama local main:
`git branch -d main`
Si por alguna razón la rama main no se ha fusionado completamente, puedes forzar la eliminación con:
`git branch -D main`
7. Actualiza tus referencias locales:
Para asegurarte de que las ramas eliminadas ya no aparecen en tus referencias locales, ejecuta:
`git fetch --prune`

### Resumen de pasos:
1. Asegúrate de estar en main: git checkout main.
2. Actualiza tu repositorio local con los cambios remotos: git fetch origin.
3. Fusiona main con master: git merge origin/master.
4. Sube los cambios a main (si es necesario): git push origin main.
5. Fusiona main en master en el repositorio remoto (desde terminal o mediante Pull Request).
6. Elimina las ramas locales y remotas main: git push origin --delete main y git branch -D main.
7. Actualiza las referencias locales: git fetch --prune.
8. Con estos pasos, habrás fusionado tu rama main con master y eliminado la rama no deseada. ¡Espero que esta explicación te sea útil! Si tienes más dudas, no dudes en preguntar.