
#
* IMPORTANTE: puede instalar una instancia de vtiger desde 0 directo en docker (con el wizard de vtiger), pero se recomienda que lo haga fuera de docker y luego la agregue a docker.
# 
1- Descomprime el código fuente de vtiger dentro de la carpeta **vtigercrm**

2- Configura las variables de entorno para mysql
#

`
MYSQL_HOST=mysql
MYSQL_ROOT_PASSWORD=SuperSecret
MYSQL_DATABASE=vtiger
MYSQL_USER=vtiger
MYSQL_PASSWORD=secret
`

#
*Si instalaste vtiger desde el contenedor, puedes copiar el archivo config.inc.php a tu carpeta local luego de desplegar el contenedor con el siguiente comando:*

-- donde **1a622d65c3cf** es el ID del contenedor (NO se usa el nombre)
> docker cp 1a622d65c3cf:/var/www/html/config.inc.php vtigercrm/

*También puedes usar el archivo example-config.inc.php para que puedas usar variables de entorno en vtiger*
#
Para usar variables de entorno necesita las siguientes variables de entorno:

`
VT_SITE_URL=http://vtiger.misitioweb.com/
MYSQL_HOST=mysql
MYSQL_ROOT_PASSWORD=SuperSecret
MYSQL_DATABASE=vtiger
MYSQL_USER=vtiger
MYSQL_PASSWORD=secret
`
#
- Construir y desplegar imagen
> docker build -t hcumbicusr/vtiger:7.5.0 .
#
> docker-compose up -d


# 
- Para sacar backup de la bd dentro del contenedor, se guardará la ruta actual del host
> docker exec mysql80 mysqldump -uroot -pSuperSecret vtiger > backup_vtiger.sql

