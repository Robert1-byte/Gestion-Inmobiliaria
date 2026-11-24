# Diagrama de la arquitectura inicial

```mermaid
flowchart LR
    U["Navegador<br/>del usuario"] --> A["Apache2 existente<br/>80/443"]
    A --> FE["Frontend estático<br/>Nginx"]

    subgraph DC["Docker Compose"]
        FE --> API["API REST / OpenAPI<br/>Spring Boot 3.x · Java 21"]
        API --> SEC["Seguridad JWT<br/>ADMIN / CONTADOR"]
        SEC --> MOD["Módulos I1<br/>Propiedades<br/>Contratos<br/>Inventario<br/>Finanzas"]
        MOD --> INF["Infraestructura<br/>JPA · Flyway · Storage PDF"]
        INF --> DB[("PostgreSQL 17<br/>volumen pgdata")]
        INF --> VOL[("Volumen contracts<br/>PDF fuera de la BD")]
    end

    CI["GitHub Actions<br/>backend-ci"] -.-> API
    DO["DigitalOcean VPS<br/>despliegue previsto I2"] -.-> DC
```

La versión editable en Mermaid está en este archivo; la imagen PNG/SVG equivalente se publica como `docs/diagrama-arquitectura.svg`.

Notas:

- Apache2 ya ocupa los puertos 80 y 443; no se modifica en la Actividad 1.
- El backend se expone temporalmente en el puerto 8080 dentro de la red de Docker.
- Los contratos y comprobantes PDF se guardan en un volumen persistente, nunca en PostgreSQL.
- GitHub Actions ejecuta `backend-ci` antes de fusionar cambios en `main`.
