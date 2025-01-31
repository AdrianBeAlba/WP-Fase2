# Importación de contenido y restauración de copias de seguridad.

# Importación de templates para tu pagina web

## Paso 1 descargar templates

1. Nos dirigimos a Wordpress > Apariencia > Temas > Añadir nuevo tema
2. Buscamos el tema deseado en nuestro caso ***Twenty Twenty-Two***
3. Revisamos la info y previsualización del tema y le damos a ***Instalar***

## Paso 2 Activar y modificar tema

1. Nos dirigimos a Wordpress > Apariencia > Temas, podremos ver que nuestro nuevo tema aparece en la lista.
2. Movemos el cursor encima y pulsamos en ***Activar***
3. Nuestro tema queda instaurado en nuestra pagina.

***Nota: realizar esta operación en una pagina con contenido puede afectar negativamente al mismo***

# Restauración de copias de seguridad

## Restauración de ficheros

### Paso 1 Extrae la copia de seguridad
~~~bash
unzip wordpress_files_YYYY-MM-DD_HH-MM-SS.zip -d wordpress_backups/
~~~
### Paso 2 Copia los archivos al contenedor de WordPress

~~~bash
docker cp /home/wp-admin/wordpress_backups/wordpress_files/. wordpress-app:/var/www/html
~~~

### Paso 3 Reconfigurar permisos
~~~bash
docker exec wordpress-app chown -R www-data:www-data /var/www/html
~~~

## Restauración de la base de datos
***Nota: necesitamos tener instalado el cliente de mariadb***
### Paso 1 Extrae la copia de seguridad
~~~bash
unzip /home/wp-admin/wordpress_backups/wordpress_db_YYYY-MM-DD_HH-MM-SS.zip -d /home/wp-admin/wordpress_backups/
~~~

### Paso 2 Restauracion de la bd
~~~bash
docker exec -i wordpress-db mariadb -uwordpress_user -ppassword123 wordpress < wordpress_db_2025-01-31_10-01-10.sql
~~~

