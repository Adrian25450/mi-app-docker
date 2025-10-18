🚀 mi-app-docker
Aplicación PHP desplegada automáticamente con Jenkins y Docker.

📦 Estructura del Proyecto
Código
mi-app-docker/
├── Dockerfile
├── Jenkinsfile
├── README.md
├── app/
└── index.php
🛠️ Tecnologías Utilizadas
Docker: Contenerización de la aplicación.

Jenkins: Automatización del pipeline CI/CD.

GitHub: Repositorio de código fuente.

PHP 8.2 + Apache: Stack de servidor web.

Slack (opcional): Notificaciones de pipeline.

⚙️ Pipeline CI/CD
El pipeline automatiza los siguientes pasos:

Checkout del repositorio desde GitHub.

Build de la imagen Docker con versión dinámica (BUILD_VERSION).

Test dentro del contenedor para verificar que PHP esté activo.

Push de la imagen a Docker Hub (adrian0526/mi-app-docker).

Deploy en entorno de prueba (localhost:8081).

📄 Dockerfile
Dockerfile
FROM php:8.2-apache

ARG BUILD_VERSION
ENV BUILD_VERSION=$BUILD_VERSION

COPY app/ /var/www/html/

EXPOSE 80
📄 Jenkinsfile
El Jenkinsfile define el pipeline completo, incluyendo etapas de build, test, push y despliegue. Las credenciales de Docker Hub se configuran en Jenkins como docker-hub-credentials.

🌐 Webhooks
⚠️ GitHub no permite configurar Webhooks con localhost como URL de destino. Se recomienda usar una URL pública o túnel (ej. ngrok) para pruebas locales.

🧪 Verificación
Una vez desplegada, accede a http://localhost:8081 para ver la aplicación funcionando:

html
<h1>¡Hola Mundo desde Docker!</h1>
<p>Desplegado automáticamente con Jenkins</p>
<p>Versión: ${BUILD_ID}</p>
