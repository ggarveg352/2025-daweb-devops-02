# Control de versiones y CI/CD - Parte II

Esta tarea consiste en un Pipeline automatizado que construye imágenes Docker, las sube a DockerHub y despliega en AWS EC2 mediante GitHub Actions.

---

## 1. Configuración Local

### 1.1. Preparar entorno

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 1.2. Probar aplicación

```bash
python src/app.py
curl http://localhost:5000

# Con Gunicorn
gunicorn --chdir src app:app --bind 0.0.0.0:5000
```

---

## 2. Dockerización

### 2.1. Construir y probar imagen

```bash
docker build -t <usuariodockerhub>/galeria:latest .
docker run -d -p 80:5000 --name testgaleria <usuariodockerhub>/galeria
docker stop testgaleria && docker rm testgaleria
```

### 2.2. Configurar docker-compose.yml

### 2.3. Comandos Docker Compose

```bash
docker compose up -d
docker compose down
docker push <usuariodockerhub>/galeria:latest
```

---

## 3. GitHub Actions - Build & Push

### 3.1. Configurar Secretos en GitHub

Settings → Security → Secrets and Variables → Actions

**Secretos para DockerHub:**
- DOCKERHUB_USERNAME: Usuario Docker Hub
- DOCKERHUB_TOKE`: Token desde Docker Hub (Account Settings/Security)

**Secretos para AWS:**
- AWS_USERNAME: Usuario EC2 (ubuntu/admin)
- AWS_HOSTNAME: IP pública EC2
- AWS_PRIVATEKEY: Contenido completo de vockey.pem

### 3.2. Crear workflow .github/workflows/.yml

### 4. Flujo de trabajo

1. Modificar código en directorio `src/`
2. Hacer git add /src
3. Hacer git commit - m "Haciendo cambios en el index"
4. Hacer push a rama `main`
5. GitHub Actions automáticamente:
   - Construye imagen Docker
   - Sube imagen a DockerHub
   - Despliega en AWS EC2
6. Verificar aplicación en `http://<IP_EC2>`

### 5.2. Monitoreo

- Ver progreso en pestaña **Actions** de GitHub
- Revisar cada job (build y aws)
- Confirmar actualización en DockerHub

Basta con volver a modificar el código en el directorio src. Esto dispara la creación de la imagen y si es correcta dispara el trabajo de despliegue en AWS EC2.
