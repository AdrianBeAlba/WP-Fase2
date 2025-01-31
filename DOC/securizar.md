# Securizar un sitio web WordPress con SSL usando Certbot

Proteger nuestro sitio web con un certificado SSL es crucial para garantizar la seguridad y privacidad de los datos. En este documento, veremos dos formas de hacerlo:

1. **Usando Docker con Certbot**.
2. **En un servidor de hosting tradicional**.

## 1. Usando Certbot en un entorno Docker

Si estamos ejecutando WordPress en un contenedor Docker, podemos utilizar Certbot para obtener y renovar certificados SSL de Let's Encrypt.

### Requisitos previos

- Tener WordPress corriendo en Docker.
- Tener un dominio apuntando al servidor.
- Contar con un servidor web como Nginx o Apache en otro contenedor.

### Paso 1: Crear un contenedor para Certbot

Ejecutamos el siguiente comando para solicitar un certificado SSL:

```bash
sudo docker run --rm -v certbot-etc:/etc/letsencrypt -v certbot-var:/var/lib/letsencrypt certbot/certbot certonly --standalone -d midominio.com -d www.midominio.com --email admin@midominio.com --agree-tos --no-eff-email
```

Esto generará un certificado en el volumen `certbot-etc`.

### Paso 2: Configurar Nginx o Apache para usar SSL

Si estamos usando **Nginx**, añadimos la configuración en nuestro archivo del sitio:

```nginx
server {
    listen 443 ssl;
    server_name midominio.com www.midominio.com;

    ssl_certificate /etc/letsencrypt/live/midominio.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/midominio.com/privkey.pem;

    location / {
        proxy_pass http://wordpress-container;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $host;
    }
}
```

Reiniciamos Nginx con:

```bash
docker restart nginx-container
```

### Paso 3: Renovación automática del certificado

Añadimos una tarea `cron` para renovar automáticamente el certificado:

```bash
0 3 * * * docker run --rm -v certbot-etc:/etc/letsencrypt -v certbot-var:/var/lib/letsencrypt certbot/certbot renew --quiet && docker restart nginx-container
```

---

## 2. Instalación en un servidor de hosting tradicional

Si usamos un servidor de hosting tradicional con acceso SSH, podemos instalar Certbot y obtener un certificado.

### Paso 1: Instalar Certbot

En servidores con Apache:

```bash
sudo apt update && sudo apt install certbot python3-certbot-apache -y
```

En servidores con Nginx:

```bash
sudo apt update && sudo apt install certbot python3-certbot-nginx -y
```

### Paso 2: Obtener el certificado

Para Apache:

```bash
sudo certbot --apache -d midominio.com -d www.midominio.com
```

Para Nginx:

```bash
sudo certbot --nginx -d midominio.com -d www.midominio.com
```

### Paso 3: Configurar la renovación automática

Certbot configura automáticamente una tarea `cron`, pero podemos verificarlo con:

```bash
sudo systemctl status certbot.timer
```

### Paso 4: Verificar HTTPS en WordPress

Ingresamos a `Ajustes > Generales` en el panel de WordPress y aseguramos que las URLs comiencen con `https://`.

---

## Conclusión

Implementar SSL en WordPress es esencial para la seguridad y SEO del sitio. Dependiendo de si estamos en un entorno Docker o en un servidor tradicional, podemos usar Certbot para obtener y gestionar certificados de manera sencilla.



