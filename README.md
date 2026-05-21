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

