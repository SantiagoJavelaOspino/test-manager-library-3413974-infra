# test-manager-library-3413974-infra

> **Prueba técnica · Ficha ADSO 3413974**  
> Metodología: **Spec-Driven Development (SDD)**

Infraestructura y orquestación de servicios para **Manager Library**.

---

## 1. Servicios Orquestados

| Servicio | Imagen / Contexto | Puerto Host | Descripción | Healthcheck |
|---|---|---|---|---|
| `db` | `postgres:16-alpine` | `5432` | Base de datos PostgreSQL con esquema `library` | `pg_isready -U postgres -d manager_library` |
| `api` | `../api` (PHP 8.2) | `8000` | Backend API REST con arquitectura hexagonal | Depende de `db` (healthy) |
| `app` | `../app` (Node build + Nginx) | `3000` | Frontend React SPA compilada para producción | Depende de `api` |
| `page` | `../page` (Nginx Alpine) | `8080` | Sitio público estático de presentación | Independiente |

---

## 2. Instrucciones de Uso

### 2.1 Preparación
```bash
# Copiar variables de entorno
cp .env.example .env
```

### 2.2 Despliegue de los Servicios
```bash
# Iniciar todos los contenedores en segundo plano
docker compose up -d

# Verificar el estado de los servicios
docker compose ps

# Monitorear logs en tiempo real
docker compose logs -f
```

### 2.3 Detención
```bash
# Detener contenedores
docker compose down

# Detener eliminando volúmenes de datos
docker compose down -v
```

---

## 3. Puertos y Accesos Locales

- **Frontend App:** [http://localhost:3000](http://localhost:3000)
- **Backend API:** [http://localhost:8000/api/v1/health](http://localhost:8000/api/v1/health)
- **Sitio Público Page:** [http://localhost:8080](http://localhost:8080)
- **PostgreSQL:** `localhost:5432` (Base de datos: `manager_library`, Esquema: `library`)

---

## 4. Redes y Volúmenes

- **Red:** `library_net` (bridge compartida entre los 4 contenedores).
- **Volumen Persistente:** `pgdata` (almacena el directorio `/var/lib/postgresql/data` del motor PostgreSQL).
