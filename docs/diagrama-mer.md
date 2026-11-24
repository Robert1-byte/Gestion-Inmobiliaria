# Diagrama entidad-relación

```mermaid
erDiagram
    USUARIO ||--o{ MOVIMIENTO : registra
    USUARIO ||--o{ CONTRATO_DOCUMENTO : carga
    USUARIO ||--o{ REPORTE : genera

    INQUILINO ||--o{ CONTRATO_ARRENDAMIENTO : firma
    ADMINISTRADOR ||--o{ CONTRATO_ADMINISTRACION : firma
    ADMINISTRADOR ||--o{ INFORME_MENSUAL : entrega
    ADMINISTRADOR ||--o{ LIQUIDACION_COMISION : recibe

    PROPIEDAD ||--o{ CONTRATO : tiene
    PROPIEDAD ||--o{ INVENTARIO_ITEM : contiene
    PROPIEDAD ||--o{ MOVIMIENTO : registra
    PROPIEDAD ||--o| CIERRE_MENSUAL : cierra
    PROPIEDAD ||--o{ INFORME_MENSUAL : recibe
    PROPIEDAD ||--o{ REPORTE : resume

    CONTRATO ||--o{ CONTRATO_DOCUMENTO : versiona_PDF
    CONTRATO ||--o| CONTRATO_ARRENDAMIENTO : especializa
    CONTRATO ||--o| CONTRATO_ADMINISTRACION : especializa

    INFORME_MENSUAL ||--|{ RESERVA : detalla
    INFORME_MENSUAL ||--o| LIQUIDACION_COMISION : genera

    USUARIO {
        bigint id PK
        string username
        string password_hash
        string rol
        boolean activo
    }
    INQUILINO {
        bigint id PK
        string nombre
        string documento
        string correo
    }
    ADMINISTRADOR {
        bigint id PK
        string nombre
        string documento
        string correo
    }
    PROPIEDAD {
        bigint id PK
        string codigo UK
        string direccion
        string tipo
        string modalidad
        string estado
    }
    CONTRATO {
        bigint id PK
        string numero
        date fecha_firma
        date fecha_inicio
        date fecha_vencimiento
        string estado
    }
    CONTRATO_ARRENDAMIENTO {
        bigint id PK
        bigint propiedad_id FK
        bigint inquilino_id FK
        numeric canon
    }
    CONTRATO_ADMINISTRACION {
        bigint id PK
        bigint propiedad_id FK
        bigint administrador_id FK
        numeric porcentaje
    }
    CONTRATO_DOCUMENTO {
        bigint id PK
        bigint contrato_id FK
        string ruta_relativa
        string sha256
        bigint tamano_bytes
    }
    INVENTARIO_ITEM {
        bigint id PK
        bigint propiedad_id FK
        string descripcion
        string categoria
        numeric precio_unitario
        int cantidad
    }
    MOVIMIENTO {
        bigint id PK
        bigint propiedad_id FK
        string tipo
        string categoria
        numeric monto
        date fecha
    }
    CIERRE_MENSUAL {
        bigint id PK
        bigint propiedad_id FK
        int mes
        int anio
        string estado
    }
    REPORTE {
        bigint id PK
        bigint propiedad_id FK
        int mes
        int anio
        string ruta_archivo
    }
    INFORME_MENSUAL {
        bigint id PK
        bigint propiedad_id FK
        bigint administrador_id FK
        int mes
        int anio
        string estado
    }
    RESERVA {
        bigint id PK
        bigint informe_id FK
        string plataforma
        date fecha_entrada
        date fecha_salida
        numeric monto
    }
    LIQUIDACION_COMISION {
        bigint id PK
        bigint informe_id FK
        numeric base
        numeric porcentaje
        numeric monto_comision
    }
```

La imagen equivalente está en `docs/diagrama-mer.svg`.

Notas:

- `CONTRATO` es la supertipo; `CONTRATO_ARRENDAMIENTO` y `CONTRATO_ADMINISTRACION` son especializaciones excluyentes.
- Los importes son `NUMERIC(14,2)`.
- Los contratos PDF no se guardan en PostgreSQL: se registra `ruta_relativa` y `sha256` en `CONTRATO_DOCUMENTO`.
- El cierre mensual bloquea movimientos e informes del mismo periodo por regla de negocio.
