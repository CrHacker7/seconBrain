# Navega a la carpeta de tu proyecto
- `cd ~/MisProyectos/ProyectoJava`
Comprimir y excluir .git y .idea: Para crear un archivo .zip y excluir específicamente los directorios .git y .idea, puedes usar el siguiente comando:
- `zip -r threadLogging * -x ".git/*" -x "*.idea/*"`
- *zip -r nombre_del_archivo.zip * -x "*.git/*" -x "*.idea/*"*
##### Explicación
1. zip: El comando para comprimir archivos.
2. -r: Recursivo, para incluir subdirectorios.
3. nombre_del_archivo.zip: El nombre que le quieras dar al archivo .zip. Por ejemplo, mi_proyecto.zip.
4. *: Comprimir todos los archivos y directorios en el directorio actual.
5. -x "*.git/*": Excluir cualquier cosa dentro de la carpeta .git.
6. -x "*.idea/*": Excluir cualquier cosa dentro de la carpeta .idea.