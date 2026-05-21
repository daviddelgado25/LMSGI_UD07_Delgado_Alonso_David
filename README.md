# Manual de Explotación — ERP/CRM WillmanTech S.L.

## 1. Introducción y Arquitectura

WillmanTech S.L. utiliza un sistema ERP para gestionar ventas, facturación y clientes. El sistema está formado por dos componentes principales que se ejecutan mediante Docker Compose:

- Servidor ERP: interfaz web accesible en http://localhost:8069.
- Base de datos (PostgreSQL): almacena todos los datos del negocio.

Los módulos activos son: Ventas, Facturación, CRM e Informes.

## 2. Guía de Instalación y Reinstalación

### Requisitos previos
- Docker y Docker Compose instalados.
- 4 GB de RAM disponibles y 20 GB de espacio en disco.

### Variables de entorno
Crear un fichero .env en la raíz del proyecto:

dotenv
POSTGRES_DB=willmantech_prod
POSTGRES_USER=odoo
POSTGRES_PASSWORD=TuContraseñaSegura
ODOO_ADMIN_PASSWD=ClaveAdministrador
ERP_PORT=8069

### Arrancar el sistema
bash

# Iniciar todos los servicios
docker compose up -d

# Comprobar que están en marcha
docker compose ps

Una vez arrancado, acceder a http://localhost:8069 y completar el asistente de configuración inicial.

### Reinstalación completa

bash
docker compose down -v   (Elimina contenedores y volúmenes)
docker compose up -d     (Vuelve a crear todo desde cero)

## 3. Seguridad y Control de Acceso

El sistema define tres perfiles de usuario:

Administrador - Acceso total: configuración, usuarios y módulos.
Contable - Gestión de facturas, pagos e informes financieros.
Comercial - Gestión de clientes, presupuestos y pedidos de venta.

### Crear un usuario
Ajustes - Usuarios - Nuevo - rellenar nombre, email y asignar el rol.

### Recomendaciones de seguridad
- Usar contraseñas de al menos 12 caracteres con mayúsculas, números y símbolos.
- Activar la verificación en dos pasos para administradores desde Ajustes → Autenticación de dos factores.
- No exponer el puerto de la base de datos (5432) fuera del servidor.

## 4. Procedimiento de Backup y Restauración

### Hacer una copia de seguridad
bash
docker exec willmantech_db \
  pg_dump --username=odoo --format=custom --compress=9 willmantech_prod \
  > ./backups/backup_$(date +%Y%m%d).dump

Este comando genera un fichero .dump comprimido con toda la base de datos.

### Restaurar una copia
bash

### 1. Parar el servidor ERP
docker compose stop web

### 2. Restaurar el fichero de backup
docker exec -i willmantech_db \
  pg_restore --username=odoo --dbname=willmantech_prod \
  < ./backups/backup_YYYYMMDD.dump

### 3. Volver a arrancar
docker compose start web

