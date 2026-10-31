# SGI-FIIS Deploy

Sistema de Gestión de Información - Facultad de Ingeniería Industrial y de Sistemas (FIIS).
Repositorio consolidado e independiente para despliegue y desarrollo.

## Estructura del Proyecto

```text
sgi-fiis-deploy/
├── backend/    # API REST con Spring Boot + PostgreSQL
├── frontend/   # SPA con React + TypeScript + Vite + Tailwind CSS
├── .gitignore
└── README.md
```

## Requisitos Previos

- **Java JDK**: 21 o superior
- **Node.js**: 18+ y npm
- **Docker & Docker Compose** (opcional, para ejecución con contenedores)
- **PostgreSQL**: 15+ (si se corre localmente)

## Configuración y Ejecución

### Backend (Spring Boot)
1. Navega a `backend/`:
   ```bash
   cd backend
   ```
2. Configura las variables de entorno:
   Copia `.env.example` a `.env` y define tus credenciales de base de datos.
3. Ejecución:
   - Con Docker:
     ```bash
     docker compose up --build
     ```
   - Localmente (dentro de `backend/fiis`):
     ```bash
     mvn clean spring-boot:run
     ```

### Frontend (React + Vite)
1. Navega a `frontend/`:
   ```bash
   cd frontend
   ```
2. Instala dependencias:
   ```bash
   npm install
   ```
3. Configura variables de entorno:
   Copia `.env.example` a `.env` y ajusta `VITE_API_URL` según tu backend.
4. Ejecuta en modo desarrollo:
   ```bash
   npm run dev
   ```
