# Arquitectura inicial

## Vista física

```text
Navegador
   ↓ Apache2 existente (80/443)
Frontend estático / Nginx
   ↓ REST/JSON
Backend Java 21 + Spring Boot 3.x
   ├── PostgreSQL 17 (volumen pgdata)
   └── Volumen /contracts (PDF fuera de la BD)
```

## Servicios Docker previstos

| Servicio | Función | Persistencia |
|---|---|---|
| `db` | PostgreSQL 17, red interna | `pgdata` |
| `backend` | API REST, JWT, reglas de negocio | — |
| `frontend` | Build estático, opcional en I1 | — |
| `contracts` | Volumen compartido para PDF | `contracts` |

El puerto 8080 se usa temporalmente; Apache2 ya ocupa 80/443 y no debe modificarse en la Actividad 1.

## Estructura del backend

```text
com.empresa.inmobiliaria
├── common/       errores, auditoría, BigDecimal, OpenAPI
├── security/     JWT y roles
├── property/     api / application / domain / infrastructure
├── contract/     api / application / domain / infrastructure
├── inventory/    api / application / domain / infrastructure
├── cashflow/     api / application / domain / infrastructure
├── storage/      guardado de PDF y SHA-256
└── config/
```

## Decisiones técnicas

- Importes: `BigDecimal` en Java y `NUMERIC(14,2)` en PostgreSQL.
- Fechas de negocio: `LocalDate`; auditoría: `OffsetDateTime`.
- Identificadores: `BIGINT IDENTITY`.
- Migraciones: Flyway.
- Validación: Bean Validation; errores: formato RFC 9457.
- Documentación: springdoc-openapi.
- Pruebas: JUnit 5, MockMvc y Testcontainers con PostgreSQL.
- Roles iniciales propuestos: `ADMIN`, `CONTADOR`. El rol `ADMINISTRADOR` se incorpora en I2.

## Alcance del incremento 1

CRUD de propiedades, contratos notarizados en PDF con control de vigencia, inventario valorizado y registro básico de ingresos y egresos. Comisiones, reportes y cierre de mes pertenecen al incremento 2.

Sustento: ISO/IEC/IEEE 12207:2026 §§6.4.4, 6.4.5, 6.4.7.
