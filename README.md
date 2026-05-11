# Innovatech Chile - Etapa 2: Despliegue y Automatización Cloud

Este repositorio contiene la solución técnica para la Etapa 2 del proyecto de la empresa **Innovatech Chile**. El objetivo principal es la contenedorización de una arquitectura de microservicios, la persistencia de datos y la implementación de un flujo de Integración y Entrega Continua (CI/CD) para su despliegue en Amazon Web Services (AWS).

## 🚀 Tecnologías Utilizadas
* **Frontend:** React / Vite.
* **Backend:** Microservicios con Java Spring Boot (Ventas y Despachos).
* **Base de Datos:** MySQL.
* **Contenedorización:** Docker & Docker Compose.
* **Infraestructura:** AWS EC2 (Instancias en subredes públicas y privadas).
* **CI/CD:** GitHub Actions.

---

## 🏗️ Arquitectura del Sistema
La solución se divide en tres componentes principales orquestados para trabajar de forma conjunta:

1. **Capa de Presentación (Frontend):** Desplegada en una instancia EC2 pública para permitir el acceso de los clientes vía navegador.
2. **Capa de Negocio (Backend):** Dos microservicios independientes que procesan la lógica de ventas y despachos, alojados en una subred privada para mayor seguridad.
3. **Capa de Datos:** Un motor MySQL que provee almacenamiento persistente a los servicios de backend.

---

## 📦 Contenedorización (Docker)
Cada servicio cuenta con un `Dockerfile` diseñado bajo el principio de **multi-stage build**:
* **Fase de Compilación:** Se utilizan imágenes de construcción (Node.js/Maven) para generar los artefactos necesarios.
* **Fase de Producción:** Se utilizan imágenes minimalistas y seguras (Nginx/JRE) ejecutadas con un **usuario no root** para minimizar riesgos de seguridad.

### Orquestación con Docker Compose
Se incluye un archivo `docker-compose.yml` que orquestra el stack completo, gestionando:
* **Redes:** Aislamiento de la comunicación entre el Backend y la BD.
* **Dependencias:** Control del orden de inicio de los servicios (`depends_on`).
* **Variables de Entorno:** Configuración de puertos y credenciales de acceso.

---

## 💾 Persistencia de Datos
Se implementó la **persistencia de datos** mediante el uso de **Named Volumes** en Docker.
* **Justificación:** Se seleccionaron volúmenes nombrados para asegurar que la información de la base de datos sea persistente ante reinicios de los contenedores, garantizando la integridad de los registros de Innovatech.

---

## 🤖 Pipeline CI/CD (GitHub Actions)
La automatización del despliegue se define en el directorio `.github/workflows/`. El pipeline se activa mediante un evento `push` en la rama **deploy**:

1. **Build:** Construcción automatizada de las imágenes Docker para Front y Back.
2. **Push:** Publicación de imágenes en el registro de contenedores (Docker Hub).
3. **Deploy:** Conexión vía SSH a la instancia EC2 para actualizar los contenedores en tiempo real.

> **Seguridad:** El manejo de credenciales de AWS, tokens de Docker y claves SSH se realiza exclusivamente a través de **GitHub Secrets**.

---

## 🛠️ Instrucciones de Ejecución Local

1. **Clonar el repositorio:**
   ```bash
   git clone [https://github.com/carlitroxxx/ev2-devops.git]

2. **Ejecutar el stack de servicios:**

Bash
docker-compose up -d

3. **Acceder a la aplicación:**

Frontend: http://localhost:80

Ventas API: http://localhost:8081

Despachos API: http://localhost:8080
