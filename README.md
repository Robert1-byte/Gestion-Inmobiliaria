# Gestión Inmobiliaria

Sistema web para centralizar propiedades, contratos, inventario valorizado, flujo de caja y rentabilidad de un portafolio inmobiliario familiar.

## Estado del proyecto

- Modelo de desarrollo: incremental evolutivo, dos incrementos.
- Incremento 1: 18/12/2026.
- Incremento 2: 01–04/02/2027.
- Backend: Java 21 + Spring Boot 3.x.
- Base de datos: PostgreSQL 17 (SQL).
- Contenedores: Docker + Docker Compose.
- Referencia normativa: ISO/IEC/IEEE 12207:2026.

## Estructura

```text
.github/                 Configuración de GitHub, plantillas y CI
backend/                 Código Java / Maven
docs/                    Acta, SRS, arquitectura, MER y trazabilidad
evidencias/              Evidencias de actividad e incrementos
docker-compose.yml       Orquestación local y despliegue
```

## Convenciones

- `main`: rama protegida; todo cambio entra por PR.
- Ramas de trabajo: `feature/I1-<modulo>`, `fix/I1-<id>`, `docs/<artefacto>`, `infra/<tema>`.
- Commits: `<tipo>(<alcance>): <descripción>`, por ejemplo `feat(propiedades): alta de propiedad`.
- Cada issue debe referenciar `HU-XX`, `RF-XX` o `RN-XX` según corresponda.
- No subir secretos, `.env`, PDF de contratos ni datos personales reales.

## Documentación

- [Plan de configuración y administración](docs/plan-configuracion.md)
- [Arquitectura inicial](docs/arquitectura-inicial.md)
- [Modelo conceptual de base de datos](docs/modelo-conceptual.md)
- [Matriz de trazabilidad (Excel)](docs/matriz-trazabilidad.xlsx)

## Puesta en marcha

```bash
cp .env.example .env
docker compose up --build
```

La ABI pública está prevista en `/api/v1` y la documentación OpenAPI en `/swagger-ui.html`.
