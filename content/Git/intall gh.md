######## DESCARGAR GH #######################
PRIMERO HACER GIT CONFIG ANTES DE INSTALAR GH

(type -p wget >/dev/null || (sudo apt update && sudo apt-get install wget -y)) \
&& sudo mkdir -p -m 755 /etc/apt/keyrings \
&& wget -qO- https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
&& sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
&& sudo apt update \
&& sudo apt install gh -y
_______________________________________________________________________________
GH INSTALL

git config user.name "name"
git config user.mail "mail@mail.com"
git config user.password "4WTVEGsdfsdfsd" //optional

________________________________CREAR REPO DESDE 0____________________________
1. gh auth status
1. gh --version
1. gh auth login

2. git clone --mirror <URL> // clonar todas sus branches

3. git init
3. git config user.name "name"
3. git config user.email "email@mail.com"

4. gh repo create <AssistantAI --public> // si no tengo repo en github

5. git remote add origin https://github.com...git //LINK QUE ACABO DE CREAR CON GH
	git remote -v (ver remoto linked)
6. git add . &&
6. git commit -m "first commit" &&

7. git push -u origin master 



__________________________CLONA UNA RAMA_________________________________________
git clone --branch nombre-de-rama --single-branch https://....git


__________________________CAMBIAR DE RAMA________________________________________
git stash -u // guarda cambios sin confirmarlos, luego me permite cambiar de rama
git stash pop // los recupera
git stash drop //elimina
__________________________________SUBMODULOS_______________________________________

SI APARECE LA FLECHITA EN MI REPO CLONADO ES PORQUE TIENE SUBMODULOS
Para clonar un repositorio que contiene submódulos, debes usar la opción --recurse-submodules con el comando git clone, ejemplo:
-git clone --recurse-submodules https://github.com/usuario/repositorio.git

Si ya has clonado el repositorio sin usar la opción --recurse-submodules, comando para inicializar y actualizar los submódulos:
-git submodule update --init --recursive

______________________________SNAP_____________________________________________________

sudo snap refresh --classic --no-backup //me actulizará sin que me guarde los archivos antiguos
sudo snap refresh <gnome-calculator> --classic --no-backup 
sudo apt update //no usarlo mucho porque me saldran multiples archivos 
sudo apt install gh



________________________________COMMITS____________________________________________
eliminar un commit y volver a recuperarlo(muy dificil si han pasado varios commits)
-git reset --hard HEAD~1 (el ide también se vuelve al último commit)
-git reflog (se ven todos los commits, también los eliminados)
-git checkout 3393dbd (hash del commit a recuperar, estado "detached HEAD")
-git stash (para que me deje cambiar al punto del commit)
-git switch -c <new name> (rama empieza desde el commit, no desde master)
-git add .
-git commit -m "recuperando commit eliminado"
-git checkout <rama que estaba trabajando>

