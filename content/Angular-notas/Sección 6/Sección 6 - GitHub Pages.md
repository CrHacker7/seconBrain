### Solo para páginas estáticas
==Mejor crear un repo nuevo o eliminar la carpeta .git para no crear conflictos==
1. corre directamente del angular CLI
2. `npm run build`
3. **Usa CLI basado en el ==script== package.json**
4. `ng build` 
5. En mi repositorio de github
	1. settings => pages => Deploy from a branch
	1. marter => /docs (no existe todavía en mi código)
6. Renombrar carpeta **browser** por **docs** que contiene todos los archivos de **dist**
	1. Quedaría *docs >> 3rdparty/favicon/index/main/polyfills/styles*
7. mover docs al nivel de `.angular , public, src`
8. commitear todo y push
```
//copiar todo de un solo comando
	1. git remote add origin https://github.com/crhacker7/deploy-angular-web.git
	2. git branch -M main
	3. git push -u origin main
	
```
9. volver a paso 5
10. modificar el index.html de docs, añadiendo un punto al href **==.==**/
11. https://crhacker7.github.io/deploy-angular-web/
12. si vuelvo a ejecutar `ng build` se sobreescribe