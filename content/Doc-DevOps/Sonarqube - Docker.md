Instalción
1. `docker run -d --name sonarqube -e SONAR_ES_BOOSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:8.5.1-community`
2. `docker ps` -> ver contenedores
3. docker exec -it `<CONTAINER ID>` bash
4. `ls -l` -> vemos lo que tiene dentro de ese container
5. `exit`
6. en un browser -> localhost:9000
7. `cd Sonarqube/; touch Dockerfile` -> crea un dir y un file
8. `code .` -> abre el dockerfile que venimos de crear
9. Ahora copiamos el sonar.properties del contenedor a un fichero

> [!summary] terminal
> **docker exec -it 9cacdd61e697 bash**
bash-5.0# **pwd**
/opt/sonarqube
bash-5.0# **cd conf/**
bash-5.0# **ls -l**
total 24
-rw-r--r--    1 sonarqub sonarqub     20245 Oct 22  2020 *sonar.properties*
-rw-r--r--    1 sonarqub sonarqub      3217 Oct 22  2020 wrapper.conf
bash-5.0# **cat sonar.properties** 

copiamos todo el contenido de sonar.properties, también podemos descomentar y customizar como queramos
10. Descomentamos el puerto 9000 y lo cambiamos a 9090
11. Añadimos EXPOSE 9090 en el Dockerfile
12. Verificar que exista el sonar.properties en dir SOnarqube/
13. Guardar Dockerfile y sonar.properties
14. `docker image build -t sonarqube-custom .`validando con el nuevo puerto
15. `docker images` Verificar la imagen -> habrán 2 la de community y la custom
16. cambiamos 3 cosas el puerto y nombre e imagen -> `docker run -d --name sonarqube-custom -e SONAR_ES_BOOSTRAP_CHECKS_DISABLE=true -p 9090:9090 sonarqube-custom:latest` 
17. `docker ps` -> veremos dos imágenes, y si entramos en le conf/ veremos que se ha efectuado el cambio del puerto en la nueva imagen. También en el browser.