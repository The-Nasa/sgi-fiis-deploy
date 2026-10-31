# SGI-FIIS Deploy (AWS & Docker)

Sistema de Gestión de Información - FIIS.
Repositorio independiente y autocontenido para despliegue y desarrollo en la nube (AWS).

---

## Estructura del Repositorio

```text
sgi-fiis-deploy/
├── .github/workflows/
│   └── ci-cd.yml       # Pipeline automatizado de CI/CD para AWS
├── backend/            # API REST (Spring Boot 3 + Java 17 + PostgreSQL)
│   ├── fiis/           # Código fuente y migraciones Flyway
│   └── Dockerfile      # Multi-stage build con Maven
├── frontend/           # SPA (React 18 + Vite + Tailwind + Nginx)
│   ├── src/            # Código fuente React
│   ├── nginx.conf      # Servidor estático + Reverse proxy a /api
│   └── Dockerfile      # Multi-stage build con Node 20 y Nginx
├── docker-compose.yml  # Orquestador unificado de servicios
├── .env.example        # Plantilla de variables de entorno
├── .gitignore
└── README.md
```

---

## 🚀 Despliegue Rápido con Docker Compose (Local o Servidor)

Tanto en tu máquina local como en una instancia **AWS EC2**, levanta todo el sistema con un solo comando:

1. **Configurar variables de entorno:**
   ```bash
   cp .env.example .env
   ```
   *(Opcional: edita contraseñas o puertos en `.env` si lo deseas).*

2. **Construir y levantar todos los servicios:**
   ```bash
   docker compose up -d --build
   ```

3. **Acceder a la aplicación:**
   - **Frontend (Web):** `http://localhost` (puerto 80) o la IP pública de tu servidor AWS.
   - **Backend API:** `http://localhost:8080/api`
   - **Base de Datos PostgreSQL:** Puerto interno `5432` (Flyway migrará e insertará las semillas automáticamente).

4. **Ver logs o estado:**
   ```bash
   docker compose ps
   docker compose logs -f
   ```

5. **Detener servicios:**
   ```bash
   docker compose down
   ```

---

## ☁️ Guía de Despliegue en AWS (EC2)

### Paso 1: Crear la instancia EC2
1. En AWS Console, lanza una instancia EC2 (tipo recomendado: `t3.small` o `t3.medium`, sistema operativo **Ubuntu 22.04 LTS** o **24.04 LTS**).
2. Descarga tu llave privada (`.pem`).
3. En el **Security Group**, habilita los puertos de entrada:
   - **SSH (22)**: Desde tu IP (o desde cualquier IP para GitHub Actions).
   - **HTTP (80)**: Desde cualquier IP (`0.0.0.0/0`).
   - **Custom TCP (8080)**: Opcional si deseas probar la API directamente.

### Paso 2: Instalar Docker en la máquina EC2
Conéctate por SSH a tu instancia:
```bash
ssh -i tu-llave.pem ubuntu@IP_PUBLICA_EC2
```
Instala Docker y Docker Compose:
```bash
sudo apt-get update
sudo apt-get install -y docker.io docker-compose-v2
sudo usermod -aG docker $USER
newgrp docker
```

### Paso 3: Clonar y correr en EC2
```bash
git clone https://github.com/The-Nasa/sgi-fiis-deploy.git
cd sgi-fiis-deploy
cp .env.example .env
docker compose up -d --build
```
¡Listo! Abre `http://IP_PUBLICA_EC2` en tu navegador y verás la aplicación funcionando.

---

## 🔄 Automatización CI/CD con GitHub Actions

El repositorio cuenta con `.github/workflows/ci-cd.yml`:
- En cada `git push` a `main`, compila y valida tanto el **Frontend** con Vite como el **Backend** con Maven.
- Si deseas que haga despliegue automático a tu instancia EC2, agrega en **GitHub > Settings > Secrets and variables > Actions**:
  - `AWS_HOST`: IP pública de tu EC2.
  - `AWS_USER`: `ubuntu`
  - `AWS_SSH_KEY`: Contenido completo de tu archivo `.pem`.
