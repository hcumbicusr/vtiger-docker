
#
* IMPORTANTE: puede instalar una instancia de vtiger desde 0 directo en docker (con el wizard de vtiger), pero se recomienda que lo haga fuera de docker y luego la agregue a docker.
# 
1- Descomprime el código fuente de vtiger dentro de la carpeta **vtigercrm**

2- Configura las variables de entorno para mysql
#

```
VT_SITE_URL=http://vtiger.misitioweb.com/
MYSQL_HOST=mysql
MYSQL_ROOT_PASSWORD=SuperSecret
MYSQL_DATABASE=vtiger
MYSQL_USER=vtiger
MYSQL_PASSWORD=secret
```
#
- Construir y desplegar imagen
> docker build -t hcumbicusr/vtiger:7.5.0 .

# 
- Para descargar imagen y ejecutar solo docker compose
> docker-compose pull

#
> docker-compose up -d


# 
- Para sacar backup de la bd dentro del contenedor, se guardará la ruta actual del host
> docker exec mysql80 mysqldump -uroot -pSuperSecret vtiger > backup_vtiger.sql

