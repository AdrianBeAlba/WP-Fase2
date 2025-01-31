# Cómo actualizar nuestro CMS

Actualizar nuestro CMS en caso de haber actualizaciones es muy simple. Hay varias formas de hacerlo, y las explicaremos en orden.

## 1ª Forma: Actualización desde el panel

Lo primero es comprobar que hay actualizaciones.

Si el sistema está actualizado, aparecerá esto:

![img1](/WP-Fase2/IMG/ca1.png)


En caso de no estar actualizado, se mostrará algo como esto:

![img2](/WP-Fase2/IMG/ca2.png)

Si es una actualización menor (cambios pequeños), el mensaje indicará la versión disponible. Si es una actualización grande, aparecerá algo similar a esto:

![img3](/WP-Fase2/IMG/ca3.png)


También puedes descargar la última versión de la actualización de WordPress desde el hPanel:

1. Accede a tu cuenta.
2. Ve a **Tablero** bajo la sección de WordPress.
3. Desplázate hacia abajo y verás la opción **Instalar la última versión de WordPress**.
4. Haz clic en **Actualizar** para comenzar el proceso.

## 2ª Forma: Actualización vía FTP

1. Descarga la última versión de WordPress y descomprime el archivo ZIP.
2. Accede a la carpeta extraída y elimina el archivo `wp-config-sample.php` y la carpeta `wp-content` para no perder datos importantes.
3. Accede a tu sitio de WordPress a través de **FileZilla** o tu cliente FTP preferido.
4. En el lado derecho del panel (sitio remoto), busca los directorios `wp-includes` y `wp-admin`, haz clic derecho y selecciona **Borrar** para eliminarlos.
![img4](/WP-Fase2/IMG/ca4.png)


Luego, subimos dos carpetas importantes:

- `wp-includes`
- `wp-admin`

![img5](/WP-Fase2/IMG/ca5.png)


Después:

5. Sube el resto de los archivos de WordPress descomprimidos desde tu ordenador al directorio del alojamiento web.
6. Sobrescribe los archivos antiguos de WordPress subiendo los nuevos al servidor.
7. Cuando aparezca el mensaje de confirmación **El archivo de destino ya existe**, selecciona las opciones **Usar siempre esta acción** y **Aplicar solamente a la cola actual**.
8. Cuando todos los archivos estén subidos, ve a `tusitio.com/wp-admin/upgrade.php/` para comprobar si has realizado correctamente la actualización.

⚠️ **Advertencia:** Haz esto con calma y cuidado, ya que podrías afectar todo el sitio web si algo sale mal.

## 3ª Forma: Actualización con WP-CLI

Este método se realiza a través de **SSH**, por lo que debes tener acceso al servidor donde se aloja tu WordPress.

1. Accede al directorio raíz de WordPress.
2. Ejecuta el siguiente comando:
   ```bash
   wp core check-update
   ```
3. Si no hay actualizaciones, verás el siguiente mensaje:
   ```bash
   Success: WordPress updated successfully
   ```
4. Si hay una nueva versión de WordPress disponible, se mostrará algo similar a:
   ```
   +---------+-------------+---------------------------------------------------------------+
   | version | update_type | package_url                                                 |
   +---------+-------------+---------------------------------------------------------------+
   | #.#.#   | minor       | https://downloads.wordpress.org/release/wordpress-#.#.#.zip |
   | #.#.#   | major       | https://downloads.wordpress.org/release/wordpress-#.#.#.zip |
   +---------+-------------+---------------------------------------------------------------+
   ```
5. Para actualizar, ejecuta los siguientes comandos:
   ```bash
   wp core update
   wp core update-db
   wp theme update --all
   wp plugin update --all
   ```
   ![img6](/WP-Fase2/IMG/ca6.png)

6. Una vez completada la actualización, deberías ver un mensaje de éxito.

## Actualización de plugins y temas

1. Ve a **Escritorio** y haz clic en **Actualizaciones**.
![img7](/WP-Fase2/IMG/ca7.png)

2. Si hay actualizaciones disponibles, aparecerán listadas.
![img8](/WP-Fase2/IMG/ca8.png)

3. Si no hay actualizaciones, no se mostrará nada.

![img9](/WP-Fase2/IMG/ca9.png)


Hasta aquí todo. ¡Recuerda mantener tu WordPress actualizado para evitar problemas de seguridad y mejorar el rendimiento! 🚀
