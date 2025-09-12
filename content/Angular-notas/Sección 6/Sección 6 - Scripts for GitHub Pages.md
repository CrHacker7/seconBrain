### package.json
1. #### Añadir el href correctamente
	1. `"build:href": "ng build --base-href ./",` **construimos**
	2. `"npm run build:href"` **ejecutamos**
2. ####  Instalar del-cli
	1. `npm install del-cli --save-dev` **se instala como dependencia de desarrollo**
	2. `"delete:docs": "del docs",` **script**
	3. `npm run delete:docs` **ejecutamos**
3. ####  Instalar copyfiles
	1. `npm i copyfiles --save-dev`
	2. `"copy:dist": "copyfiles dist/bases/browser/* ./docs -f",` **espacio después del asterisco, -f (flat) lo aplana para que no haya jerarquía de dir**
4. ####  Ejecutar todos los cmds juntos (primero borrar dist y docs dir)
	1. `"build:github": "npm run delete:docs && npm run build:href && npm run copy:dist",`
	2. `npm run build:github` **ejecutamos**