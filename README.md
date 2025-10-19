# 🚀 mi-app-docker

Aplicación PHP desplegada automáticamente con Jenkins y Docker.

---

## 📁 Estructura del Proyecto

mi-app-docker/ 
├── Dockerfile 
├── Jenkinsfile 
├── README.md 
├── app/ 
    └── index.php

Código

---

## 🛠️ Tecnologías Utilizadas

- **Docker**: Contenerización de la aplicación.
- **Jenkins**: Automatización del pipeline CI/CD.
- **GitHub**: Repositorio de código fuente.
- **PHP 8.2 + Apache**: Stack de servidor web.
- **Slack (opcional)**: Notificaciones del pipeline.

---

## ⚙️ Pipeline CI/CD

El pipeline automatiza los siguientes pasos:

1. **Checkout** del repositorio desde GitHub.
2. **Build** de la imagen Docker con versión dinámica (`BUILD_VERSION`).
3. **Test** dentro del contenedor para verificar que PHP esté activo.
4. **Push** de la imagen a Docker Hub (`adrian0526/mi-app-docker`).
5. **Deploy** en entorno de prueba (`localhost:8081`).

---

## 📄 Dockerfile

```Dockerfile
FROM php:8.2-apache

ARG BUILD_VERSION
ENV BUILD_VERSION=$BUILD_VERSION

COPY app/ /var/www/html/

EXPOSE 80
📄 Jenkinsfile
El Jenkinsfile define el pipeline completo, incluyendo etapas de build, test, push y despliegue. Las credenciales de Docker Hub se configuran en Jenkins como docker-hub-credentials.

🌐 Webhooks
⚠️ GitHub no permite configurar Webhooks con localhost como URL de destino. Se recomienda usar una URL pública o túnel como ngrok para pruebas locales.

🧪 Verificación
Una vez desplegada, accede a http://localhost:8081 para ver la aplicación funcionando:

html
<h1>¡Hola Mundo desde Docker!</h1>
<p>Desplegado automáticamente con Jenkins</p>
<p>Versión: ${BUILD_ID}</p>
✅ Requisitos Previos
Cuenta en Docker Hub

Jenkins con los siguientes plugins instalados:

Docker Pipeline

GitHub Integration

Credentials Binding

Pipeline

Slack Notification (opcional)

📌 Notas
Las credenciales de Docker Hub deben configurarse en Jenkins como tipo Username & Password y referenciarse como docker-hub-credentials.

El puerto de despliegue local es 8081.

📣 Autor
Adrian25450 GitHub: github.com/Adrian25450
