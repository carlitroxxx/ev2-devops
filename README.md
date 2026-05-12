# Innovatech Chile - Etapa 2: Despliegue y Automatización Cloud

Este repositorio contiene la solución técnica desarrollada para la Etapa 2 del proyecto de **Innovatech Chile**.  
La solución implementa una arquitectura basada en microservicios contenedorizados utilizando Docker, automatización CI/CD con GitHub Actions y despliegue en Amazon Web Services (AWS).

---

# Tecnologías Utilizadas

- **Frontend:** React + Vite
- **Backend:** Java Spring Boot
- **Base de Datos:** MySQL 8
- **Contenedorización:** Docker & Docker Compose
- **Proxy Inverso:** Nginx
- **Infraestructura Cloud:** AWS EC2
- **Automatización CI/CD:** GitHub Actions
- **Registro de Imágenes:** Docker Hub

---

# Arquitectura del Sistema

La arquitectura se compone de dos instancias EC2 dentro de AWS:

## Instancia Pública
Contiene:
- Frontend React desplegado en contenedor Docker
- Nginx como Reverse Proxy
- Acceso desde internet

## Instancia Privada
Contiene:
- Microservicio Ventas
- Microservicio Despachos
- Base de Datos MySQL
- Acceso únicamente desde la red privada interna

## Flujo de Comunicación

```text
Cliente Web
     ↓
EC2 Pública (Nginx + Frontend)
     ↓
Reverse Proxy Nginx
     ↓
EC2 Privada (Microservicios + MySQL)
```

La implementación de Nginx permite que el frontend pueda consumir los microservicios privados sin exponer directamente los servicios backend a internet.

---

# Contenedorización con Docker

Cada componente del sistema posee su propio `Dockerfile`.

## Backend Spring Boot

Los microservicios fueron construidos utilizando:
- Maven
- Multi-stage build
- Imágenes Java optimizadas para producción

## Frontend React

El frontend fue compilado utilizando:
- Node.js
- Vite
- Nginx para servir archivos estáticos

---

# Docker Compose

Se utilizaron archivos `docker-compose.yml` para orquestar los servicios.

Las configuraciones incluyen:
- Redes Docker
- Variables de entorno
- Persistencia de datos
- Dependencias entre servicios
- Mapeo de puertos

---

# Persistencia de Datos

La persistencia de MySQL se implementó mediante **Docker Volumes**.

## Beneficios
- Mantener información aunque los contenedores se reinicien
- Separar almacenamiento de la vida útil del contenedor
- Facilitar administración de datos

---

# Reverse Proxy con Nginx

Nginx fue implementado en la instancia pública como proxy inverso.

## Funciones principales

- Recibir solicitudes HTTP desde internet
- Redirigir tráfico hacia frontend React
- Redirigir rutas `/api` hacia los microservicios privados
- Ocultar la infraestructura backend interna

## Ejemplo de rutas

```text
/api/v1/ventas
/api/v1/despachos
```

---

# Integración y Despliegue Continuo (CI/CD)

La automatización se implementó utilizando GitHub Actions.

## Flujo Automatizado

1. Build automático de imágenes Docker
2. Push de imágenes hacia Docker Hub
3. Conexión SSH hacia instancias EC2
4. Actualización automática de contenedores

## Seguridad

Las credenciales se manejan mediante:
- GitHub Secrets
- Claves SSH
- Variables de entorno seguras

---

# Infraestructura AWS

## Componentes utilizados

- VPC personalizada
- Subred pública
- Subred privada
- Security Groups
- EC2
- Elastic IP
- NAT Gateway

---

# Ejecución Local

## 1. Clonar repositorio

```bash
git clone https://github.com/carlitroxxx/ev2-devops.git
```

## 2. Ejecutar servicios

```bash
docker compose up -d
```

## 3. Acceder a la aplicación

### Frontend

```text
http://localhost
```

### API Ventas

```text
http://localhost/api/v1/ventas
```

### API Despachos

```text
http://localhost/api/v1/despachos
```

---
