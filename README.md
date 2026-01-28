# Docker Compose - Práctica de Despliegue

## Descripción

Este proyecto implementa una infraestructura completa utilizando Docker Compose con múltiples servicios integrados para una tienda en línea con PrestaShop.

## Configuración del Docker Compose

El archivo `compose.yml` define los siguientes servicios:

### 1. **MySQL Database**
- **Imagen**: mysql:9.1
- **Contenedor**: mysql_db
- **Puerto**: 3306
- **Descripción**: Base de datos relacional para almacenar la información de PrestaShop
- **Variables de Entorno**:
  - `MYSQL_ROOT_PASSWORD`: Contraseña del usuario root (desde .env)
  - `MYSQL_DATABASE`: Nombre de la base de datos
  - `MYSQL_USER`: Usuario de base de datos
  - `MYSQL_PASSWORD`: Contraseña del usuario
- **Volumen**: `mysql_data` - Persistencia de datos de la BD
- **Reinicio**: Always (reinicia automáticamente si falla)
- **Red**: backend-network

### 2. **PhpMyAdmin**
- **Imagen**: phpmyadmin:5.2.1
- **Contenedor**: phpmyadmin_panel
- **Puerto**: 8080
- **Descripción**: Panel de administración web para MySQL
- **Variables de Entorno**:
  - `PMA_ARBITRARY=1`: Permite conexión arbitraria a servidores
  - `PMA_HOST=mysql`: Host de la base de datos
- **Dependencia**: Requiere que MySQL esté activo
- **Redes**: backend-network (conexión a BD) y frontend-network (acceso web)

### 3. **PrestaShop E-commerce**
- **Imagen**: prestashop/prestashop:8
- **Contenedor**: prestashop_app
- **Puerto**: 8081
- **Descripción**: Plataforma de e-commerce para la tienda en línea
- **Variables de Entorno**:
  - `DB_SERVER=mysql`: Conexión a la base de datos
  - `DB_NAME`, `DB_USER`, `DB_PASS`: Credenciales de BD (desde .env)
- **Volumen**: `prestashop_data` - Persistencia de archivos de la aplicación
- **Dependencia**: Requiere que MySQL esté activo
- **Redes**: backend-network (conexión a BD) y frontend-network (acceso web)

### 4. **HTTPS Portal (Proxy Seguro)**
- **Imagen**: steveltn/https-portal:1
- **Contenedor**: https_proxy
- **Puertos**: 80 (HTTP) y 443 (HTTPS)
- **Descripción**: Proxy inverso que proporciona certificados SSL/TLS automáticos
- **Variables de Entorno**:
  - `DOMAINS`: Mapeo del dominio a PrestaShop
  - `STAGE`: Configuración de Let's Encrypt (staging/production)
- **Volumen**: `ssl_certs_data` - Almacenamiento de certificados SSL
- **Red**: frontend-network

## Volúmenes

- `mysql_data`: Persistencia de la base de datos MySQL
- `prestashop_data`: Persistencia de archivos de PrestaShop
- `ssl_certs_data`: Almacenamiento de certificados SSL/TLS

## Redes

- **backend-network**: Red interna para comunicación entre MySQL y aplicaciones (MySQL, PhpMyAdmin, PrestaShop)
- **frontend-network**: Red pública para acceso a interfaces web (PhpMyAdmin, PrestaShop, HTTPS Portal)

## Archivo .env Requerido

El proyecto requiere un archivo `.env` con las siguientes variables:

```env
MYSQL_ROOT_PASSWORD=your_root_password
MYSQL_DATABASE=prestashop_db
MYSQL_USER=prestashop_user
MYSQL_PASSWORD=your_password
DOMAIN=yourdomain.com
```

## Proceso y comandos utiles

# Instalacion de Docker 
![alt text](<f1-instalacion docker.png>)


# Creacion imagen ubuntu
![alt text](<f2-imagen ubuntu creacion.png>)

# Como ver imagenes locales
![alt text](<f3-imagenes locales.png>)

# Como borrar imagenes
![alt text](<f4-borrar imagenes.png>)

# Como correr una imagen 
![alt text](<f5-correr imagen .png>)

# Informacion y borrar contenedores
![alt text](<f-6 informacion y borrar contenedores.png>)

# Correr imagenes modificando su puerto 
![alt text](<f-7 correr imagenes con modificacion de puerto.png>)

# Levantar contenedores docker 
![alt text](<f-8 levantar contenedores docker .png>)

# Prestashop funcionando 
![alt text](<f-9 prestashop levantada.png>)

## Acceso a Servicios

- **PrestaShop**: http://localhost:8081
- **PhpMyAdmin**: http://localhost:8080
- **HTTPS Portal**: https://yourdomain.com (requiere dominio configurado)
