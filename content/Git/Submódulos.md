________________MANTENER SUBMODULOS(COMMITS) DE REPO CLONADOS______________________

Supongamos que tienes un repositorio principal llamado mi-repositorio que contiene un submódulo llamado mi-submodulo.

1. Clona el repositorio principal:

	git clone https://github.com/usuario/mi-repositorio.git

2. Navega al directorio del repositorio clonado:

	cd mi-repositorio

3. Inicializa y actualiza los submódulos:

	git submodule update --init --recursive

4. Cambia la URL del repositorio remoto:

	git remote set-url origin https://github.com/tu-usuario/tu-repositorio.git

5. Subir el repositorio a tu propio repositorio remoto:

	git push origin main

6. Actualiza las URLs de los submódulos en el archivo .gitmodules:

[submodule "mi-submodulo"]
    path = mi-submodulo
    url = https://github.com/tu-usuario/mi-submodulo.git

6.1 Editar el archivo .gitmodules:

	nano .gitmodules

7. Actualiza los submódulos:

	git submodule sync
	git submodule update --init --recursive

7.1 Actualiza las URLs de los submódulos:

   [submodule "mi-submodulo"]
    	path = mi-submodulo
    	url = https://github.com/tu-usuario/mi-submodulo.git

7.2 Actualizar las URLs:

Busca las secciones que se ven así:

[submodule "mi-submodulo"]
    path = mi-submodulo
    url = https://github.com/usuario/mi-submodulo.git

7.2.1 Cambia la URL a la URL de tu propio repositorio de submódulo:

[submodule "mi-submodulo"]
    path = mi-submodulo
    url = https://github.com/tu-usuario/mi-submodulo.git

8. Guarda y cierra el archivo.

9. Sincronizar los submódulos:

	git submodule sync

10. Actualizar los submódulos:

	git submodule update --init --recursive

11. Subir los cambios:
	git add .gitmodules mi-submodulo
   	git commit -m "Actualizar URLs de los submódulos"
    	git push origin main

Siguiendo estos pasos, deberías poder clonar un repositorio que contiene submódulos y luego subirlo a tu propio repositorio en GitHub sin problemas.

###  __________________________________SUBMODULOS_______________________________________

SI APARECE LA FLECHITA EN MI REPO CLONADO ES PORQUE TIENE SUBMODULOS
Para clonar un repositorio que contiene submódulos, debes usar la opción --recurse-submodules con el comando git clone, ejemplo:
-git clone --recurse-submodules https://github.com/usuario/repositorio.git

Si ya has clonado el repositorio sin usar la opción --recurse-submodules, comando para inicializar y actualizar los submódulos:
-git submodule update --init --recursive
