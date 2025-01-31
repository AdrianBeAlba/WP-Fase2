# 📊 Generación de Informes de Acceso en WordPress con Docker Compose

## Introducción
explicare cómo registrar y generar informes de acceso de los usuarios en una instalación de **WordPress con Docker Compose**. Se abordarán distintos métodos, incluyendo el uso de logs del contenedor, plugins de WordPress y un script PHP personalizado.

## 📦 Configuración del Entorno
Este sistema utiliza Docker Compos

## 🔍 Método 1: Usar Logs del Contenedor
# Redirigir Logs de Apache de WordPress a la Máquina Anfitriona

Este archivo describe cómo redirigir los logs de Apache desde un contenedor de WordPress a tu máquina anfitriona utilizando Docker Compose.

## Pasos para Redirigir los Logs

### 1. Modificar el archivo `docker-compose.yml`

Actualiza tu archivo `docker-compose.yml` para montar los logs de Apache del contenedor en una carpeta en tu máquina anfitriona.

```yaml
    volumes:
      - wordpress-data:/var/www/html
      - ./logs:/var/log/apache2  # Monta la carpeta de logs de Apache en ./logs en el host
    depends_on:
      - db
```

creamos la carpeta
```bash
mkdir -p ./logs
```
```bash
cat ./logs/access.log
```
```bash
tail -f ./logs/access.log
```

## 🛠️ Método 2: Usar un Plugin de WordPress
Si prefieres una solución sin tocar código, instala uno de estos plugins:

- **WP Activity Log** 📝 (Monitorea accesos y actividades de los usuarios)

![img10](/WP-Fase2/IMG/activity.png)

- **Simple History** 📜 (Guarda un historial de accesos y acciones en WordPress)
- **User Activity Log** 🔍 (Genera reportes detallados de la actividad de usuarios)

Estos permiten exportar registros en **CSV o PDF** fácilmente.

## 🖥️ Método 3: Registrar Accesos con PHP
Si deseas un registro **personalizado dentro del contenedor**, agrega el siguiente código a `functions.php` de tu tema activo:

```php
function registrar_accesos_usuario($user_login, $user) {
    $ip = $_SERVER['REMOTE_ADDR'];
    $hora = current_time('mysql');
    $usuario = $user->user_login;

    $registro = "Usuario: $usuario | IP: $ip | Fecha: $hora\n";
    file_put_contents(ABSPATH . "wp-content/accesos.log", $registro, FILE_APPEND);
}
add_action('wp_login', 'registrar_accesos_usuario', 10, 2);
```

Para ver los accesos dentro del contenedor:

```bash
docker compose exec wordpress-app cat /var/www/html/wp-content/accesos.log
```

## 📊 Exportación a CSV
Para convertir los logs en **CSV**, puedes usar este comando en **Linux** o dentro del contenedor:

```bash
grep "Usuario:" /var/www/html/wp-content/accesos.log | awk '{print $2, ",", $4, ",", $6}' > accesos.csv
```

Ahora tienes un archivo `accesos.csv` listo para su análisis. 📂

## 🚀 Conclusión
Estos métodos te permiten registrar y analizar los accesos de los usuarios en tu instalación de WordPress con Docker Compose. Puedes elegir entre logs del servidor, plugins o un script personalizado según tus necesidades.

---
### 🛠️ **¿Tienes dudas o sugerencias? ¡Contribuye al proyecto en GitHub!**

