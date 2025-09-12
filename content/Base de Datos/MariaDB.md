####CMD PARA INICIAR MARIADB
`sudo systemctl start mariadb`

####VER SI ESTA LEVANTADO
`sudo systemctl status mariadb`

####verificar si MariaDB está escuchando en el puerto predeterminado (3306)
`sudo netstat -tulpn`

####cmdPHPinstall
`sudo apt install -y php-cli php-fpm php-json php-intl php-imagick php-pdo php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath`

####SINTAXIS NANO
to copie down error and access messages:   

 `<Directory /var/www/html/nextcloud>`
	`Options +FollowSymlinks`
	`AllowOverride All`
	`Require all granted`
	`SetEnv HOME /var/www/html/nextcloud`
	
	`SetEnv HTTP_HOME /var/www/html/nextcloud`
	
	`<IfModule mod_dav.c>`
	    `Dav off`
	`</IfModule>`
`</directory>`