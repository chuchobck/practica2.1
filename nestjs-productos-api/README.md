# API de Productos — NestJS + TypeORM + PostgreSQL

CRUD de productos con persistencia real en PostgreSQL (reemplaza el arreglo en memoria de la práctica anterior), siguiendo la guía `web-practica-bd.html`.

## Requisitos

- Node.js 22+
- Docker y Docker Compose

## Levantar todo con Docker (API + PostgreSQL)

```bash
docker compose up -d --build
```

- API: http://localhost:3001/swagger
- PostgreSQL queda expuesto también en el host en el puerto `5442` (por si quieres conectarte con `psql` o un cliente gráfico).

## Desarrollo local (API fuera de Docker, PostgreSQL en Docker)

```bash
docker compose up -d postgres
cp .env.example .env   # ya existe un .env de ejemplo con localhost:5442
npm install
npm run start:dev
```

La API queda en http://localhost:3001 (puerto configurado en `.env` vía `PORT`) y Swagger en `/swagger`.

## Endpoints

| Verbo | Ruta | Descripción |
|---|---|---|
| GET | `/api/v1/productos` | Lista todos (o filtra con `?nombre=`) |
| GET | `/api/v1/productos/:id` | Obtiene uno |
| POST | `/api/v1/productos` | Crea |
| PUT | `/api/v1/productos/:id` | Reemplaza |
| PATCH | `/api/v1/productos/:id` | Actualiza el precio |
| DELETE | `/api/v1/productos/:id` | Elimina |

## Verificar que la persistencia es real

```bash
curl -X POST http://localhost:3001/api/v1/productos \
  -H "Content-Type: application/json" \
  -d '{"nombre":"Teclado mecanico","precio":45.90}'

docker compose restart api postgres   # o reinicia npm run start:dev

curl http://localhost:3001/api/v1/productos   # el producto sigue ahí
```

## Tests

```bash
npm test        # unitarios
npm run test:e2e
```

## Variables de entorno

Ver `.env.example`. `.env` nunca se sube al repositorio (ver `.gitignore`).
