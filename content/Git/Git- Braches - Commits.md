
### Cambiar nombre por defecto a la rama
git config --global init.defaultBranch <nombre>

###  ________________________________CREAR REPO DESDE 0____________________________
1. gh auth status
1. gh --version
1. gh auth login

2. git clone --mirror <URL> // clonar todas sus branches

3. git init
3. git config user.name "CRhACKER7"
3. git config user.email "crhacker7@gmail.com"

4. gh repo create <AssistantAI --public> // si no tengo repo en github

5. git remote add origin https://github.com...git //LINK QUE ACABO DE CREAR CON GH
	git remote -v (ver remoto linked)
6. git add . &&
6. git commit -m "first commit" &&

7. git push -u origin master 



  ###  _______________________________CLONA UNA RAMA____________________________________
`git clone --branch nombre-de-rama --single-branch https://....git`
**Elimina la rama localmente si ha sido fucionada, sino me dará error, y para forzarla, cambiar a  mayúscula ==-D==**
`git branch -d prueba`
 **commits de tu proyecto en orden cronológico**
 `git log --oneline --reverse`
**Vuelve al commit que quieres con el hash**
`git checkout 1234eas`   **`Estarás en un estado de detached HEAD`**
es bueno para ver el commit específico
**y se crea una rama nueva para trabajar basado en ese commit, y para no perder los cambios de antes de saltar de commit.**
`git switch -c nueva-rama`
 ### ver ramas fusionadas
 **desde la rama main**
 `git branch --merged`
 **historia de las ramas y sus fusiones (GODA)**
 `git log --graph --oneline --decorate --all`
 **Para ver el ancestro común entre dos ramas ( main y prueba)**
`git merge-base main prueba`

### SI QUEREMOS TENER UNA SOLA RAMA
**establecer (reset) la rama main al primer commit.**
**Asegúrate de estar en la rama main (la rama en la que quieras quedarte):**
`git checkout main`
**haz un hard reset al primer commit.**
`git reset --hard 1234eas` **Cambiará la rama main para que apunte al primer commit.**
**Verificar el estado **
`git log --oneline` **debería mostrar solo el primer commit**



###  __________________________CAMBIAR DE RAMA________________________________________
git stash -u // guarda cambios sin confirmarlos, luego me permite cambiar de rama
git stash pop // los recupera
git stash drop //elimina

###  ______________________________SNAP_____________________________________________________

sudo snap refresh --classic --no-backup //me actulizará sin que me guarde los archivos antiguos
sudo snap refresh <gnome-calculator> --classic --no-backup 
sudo apt update //no usarlo mucho porque me saldran multiples archivos 
sudo apt install gh

###  ________________________________COMMITS____________________________________________
eliminar un commit y volver a recuperarlo(muy dificil si han pasado varios commits)
-git reset --hard HEAD~1 (el ide también se vuelve al último commit)
-git reflog (se ven todos los commits, también los eliminados)
-git checkout 3393dbd (hash del commit a recuperar, estado "detached HEAD")
-git stash (para que me deje cambiar al punto del commit)
-git switch -c <new name> (rama empieza desde el commit, no desde master)
-git add .
-git commit -m "recuperando commit eliminado"
-git checkout <rama que estaba trabajando>

###  ____________________Git remove remote___________________________

`$ git remote -v`
### View current remotes
> origin  https://github.com/OWNER/REPOSITORY.git (fetch)
> origin  https://github.com/OWNER/REPOSITORY.git (push)
> destination  https://github.com/FORKER/REPOSITORY.git (fetch)
> destination  https://github.com/FORKER/REPOSITORY.git (push)

`$ git remote rm destination`

### Remove remote
$ git remote -v
### Verify it's gone
> origin  https://github.com/OWNER/REPOSITORY.git (fetch)
> origin  https://github.com/OWNER/REPOSITORY.git (push)

### Borrar los últimos commits
==**vuelve al último commit que hicimos **== 
`git checkout --  .`
**vuelve 3 commits atrás**  *afecta localmente*
`git reset HEAD~3` 
**actualiza el remoto**
`git push --force`
**Vuelve los commits de forma local**
`git revert <hash del commit>`