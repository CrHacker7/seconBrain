1. irnos a la carpeta 02-bases 
2. `ng serve --prod` || `ng build --configuration production`
3. Instalar `npm install --global http-server` [https://www.npmjs.com/package/http-server] 
4. inicia el servidor sin cache `http-server -p 8080 --cors -c-1`
5.  irnos a la carpeta `cd dist/bases/browser` para ejecutar `http-server -p 8080 -o`
6. si no funciona:
7. elimina la carpeta dist  `rm -rf dist/`
8. limpia la cache de angular
	`ng cache clean`
8. sino instalar `npm i live-server`

### PUERTO ISSUES
**ver qué proceso está usando el puerto 8080:**
`sudo lsof -i :8080`  || `netstat -tulpn | grep 8080`
**Ver qué puertos están en uso**
`netstat -tulpn`
**Si sabes el PID (número de proceso)**
`kill -9 PID`
**O matar cualquier proceso que use el puerto 8080**
`sudo kill $(sudo lsof -t -i:8080)`
**Alternativamente, puedes usar un puerto diferente:**
`http-server -p 8081 -o` || `http-server -p 3000 -o`|| `http-server -p 4200 -o`
**Si no quieres matar procesos, puedes:**
	- Cerrar todas las terminales y volver a abrir una nueva
	- Reiniciar tu computadora
	- Usar otro puerto como se mostró arriba