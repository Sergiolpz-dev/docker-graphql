# 🚀 Docker + GitHub Actions: Aprendizaje de CI/CD

Este repositorio es una guía práctica para entender cómo automatizar la construcción y el despliegue de imágenes de Docker utilizando **GitHub Actions**. El objetivo es aprender a gestionar versiones automáticas (Semantic Versioning) y publicar imágenes en Docker Hub.



## 🛠️ Tecnologías Aprendidas

* **Docker**: Containerización de aplicaciones y optimización de imágenes (Multi-stage builds).
* **GitHub Actions**: Automatización de flujos de trabajo (Workflows) mediante archivos YAML.
* **Semantic Versioning**: Gestión automática de etiquetas de versión (`Major.Minor.Patch`) basada en commits.
* **Docker Hub**: Almacenamiento y distribución de imágenes en la nube.

## 🏗️ Estructura del Workflow

El archivo `.github/workflows/docker-ci.yml` realiza los siguientes pasos automáticamente cada vez que haces un `push` a la rama `main`:

1.  **Checkout Code**: Descarga el código del repositorio en el servidor de ejecución de GitHub.
2.  **Git Semantic Version**: Calcula la siguiente versión del proyecto analizando los mensajes de los commits.
3.  **Docker Login**: Se autentica de forma segura en Docker Hub utilizando **Secrets** de GitHub.
4.  **Build**: Crea dos versiones de la imagen:
    * Una con el tag de la versión específica (ej: `0.0.3-prerelease1`).
    * Otra con el tag `latest` para tener siempre la versión más reciente disponible.
5.  **Push**: Sube ambas imágenes a tu repositorio de Docker Hub.

---

## 🔑 Configuración Necesaria (Secrets)

Para que el despliegue automático funcione, es necesario configurar los siguientes **Actions Secrets** en tu repositorio de GitHub (`Settings > Secrets and variables > Actions`):

| Secreto | Descripción |
| :--- | :--- |
| `DOCKER_USER` | Tu nombre de usuario de Docker Hub. |
| `DOCKER_PASSWORD` | Tu contraseña o Access Token (Recomendado) de Docker Hub. |



---

## 📝 Notas de Aprendizaje (Tips Técnicos)

### 🐳 Docker Tips
* **Uso de imágenes Alpine**: Siempre que sea posible, usamos `node:20-alpine` para que el peso de la imagen sea mínimo.
* **Orden de las Capas**: Primero copiamos el `package.json` y ejecutamos `yarn install`. Esto permite que, si solo cambias el código y no las librerías, Docker use la caché y el build sea mucho más rápido.
* **Versiones LTS**: Aprendimos que usar versiones impares (como Node 19) puede dar problemas de compatibilidad. Siempre es mejor usar versiones **LTS** (18, 20, 22).

### 🤖 GitHub Actions Tips
* **Seguridad (Secrets)**: Nunca subas credenciales al código. Los secretos de GitHub las ocultan incluso en los logs (aparecen como `***`).
* **Sintaxis del Comando Push**: A diferencia del `build`, el comando `docker push` **no** lleva un punto al final. 
* **Fetch Depth**: En el paso de checkout, usamos `fetch-depth: 0` para que la acción de versionamiento pueda leer todo el historial de commits y calcular la versión correcta.

---

## 🚀 Cómo usar este repo
1. Realiza cambios en tu aplicación local.
2. Haz un commit siguiendo las reglas de Semantic Version:
   * `feat: descripción` -> Sube el **Minor** (0.1.0).
   * `fix: descripción` -> Sube el **Patch** (0.0.2).
   * Incluir `major` en el commit -> Sube el **Major** (1.0.0).
3. Sube los cambios: `git push origin main`.
4. ¡Revisa el progreso en la pestaña **Actions** de GitHub!
