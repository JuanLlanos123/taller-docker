# Taller Docker - Catálogo y Pedidos

Dos servicios Spring Boot (Java 21) orquestados con Docker Compose.

Integrantes: Alvaro Sneider Portillo Mora, Juan David Llanos Castañeda.

## Ejecución

```bash
docker compose up --build
```

| Servicio | URL |
| --- | --- |
| Catálogo | http://localhost:8081 |
| Pedidos | http://localhost:8082 |

Pedidos consulta al catálogo usando la variable `CATALOGO_URL=http://catalogo:8081` definida en `compose.yaml`.

## Estructura

- `catalogo/` - servicio de productos (`/productos`).
- `pedidos/` - servicio de pedidos (`/pedidos`).
- `compose.yaml` - PostgreSQL 16 (`catalogo-db`) + los dos servicios.
- `documento.md` - respuestas de análisis.
- `evidencias/` - capturas del funcionamiento.
