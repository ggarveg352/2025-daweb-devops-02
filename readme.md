# Galería Flask – CI/CD con GitHub Actions, DockerHub y AWS

## 1. Descripción del proyecto

Este repositorio contiene una aplicación Python basada en Flask que expone una pequeña galería web.  
El objetivo de la práctica es montar una pipeline CI/CD para que, ante cambios en el código fuente, se cumpla el siguiente flujo:

- Construir una nueva imagen Docker de la aplicación.
- Publicar esa imagen en DockerHub.
- Desplegar automáticamente la nueva versión en una instancia AWS EC2 usando `docker compose`.