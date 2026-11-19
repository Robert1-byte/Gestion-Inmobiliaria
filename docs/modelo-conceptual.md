# Modelo conceptual de base de datos

```mermaid
erDiagram
    PROPIEDAD ||--o{ CONTRATO : tiene
    PROPIEDAD ||--o{ INVENTARIO_ITEM : contiene
    PROPIEDAD ||--o{ MOVIMIENTO : registra
    PROPIEDAD ||--o{ INFORME_MENSUAL : recibe
    PROPIEDAD ||--o{ CIERRE_MENSUAL : cierra
    INQUILINO ||--o{ CONTRATO_ARRENDAMIENTO : firma
    ADMINISTRADOR ||--o{ CONTRATO_ADMINISTRACION : firma
    ADMINISTRADOR ||--o{ INFORME_MENSUAL : entrega
    CONTRATO ||--o{ CONTRATO_DOCUMENTO : versiona_PDF
    CONTRATO ||--o| CONTRATO_ARRENDAMIENTO : especializa
    CONTRATO ||--o| CONTRATO_ADMINISTRACION : especializa
    INFORME_MENSUAL ||--|{ RESERVA : detalla
    INFORME_MENSUAL ||--o| LIQUIDACION_COMISION : genera
    USUARIO ||--o{ MOVIMIENTO : registra
    USUARIO ||--o{ CONTRATO_DOCUMENTO : carga
    USUARIO ||--o{ REPORTE : genera

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
      numeric garantia
      string estado
    }
    CONTRATO_ADMINISTRACION {
      bigint id PK
      bigint propiedad_id FK
      bigint administrador_id FK
      numeric porcentaje_comision
    }
    CONTRATO_DOCUMENTO {
      bigint id PK
      string ruta_relativa
      string sha256
      bigint tamano_bytes
    }
    INVENTARIO_ITEM {
      bigint id PK
      string descripcion
      string categoria
      int cantidad
      numeric precio_unitario
      string estado
    }
    MOVIMIENTO {
      bigint id PK
      string tipo
      string categoria
      numeric monto
      date fecha
      string origen
    }
    INFORME_MENSUAL {
      bigint id PK
      int mes
      int anio
      string estado
    }
    RESERVA {
      bigint id PK
      string plataforma
      numeric monto
      date fecha_inicio
      date fecha_fin
    }
    CIERRE_MENSUAL {
      bigint id PK
      int mes
      int anio
      string estado
      timestamp fecha_cierre
    }
```

## Reglas de integridad

- `UNIQUE(propiedad_id, mes, anio)` en `CIERRE_MENSUAL` e `INFORME_MENSUAL`.
- `CHECK (porcentaje_comision IN (20,25,30))` según el caso de estudio.
- `fecha_inicio <= fecha_vencimiento`.
- Foreign keys financieras con `ON DELETE RESTRICT`.
- Importes en `NUMERIC(14,2)`; nunca binarios PDF dentro de PostgreSQL.
- Valor activo de inventario = `SUM(precio_unitario * cantidad)` excluyendo `DAÑADO` y `DADO_DE_BAJA`.
- El cierre de mes bloquea cambios de movimientos e informes del mismo periodo.

## Puntos pendientes de validación con el cliente

1. Base exacta para calcular la comisión de 20 %, 25 % o 30 %.
2. Regla para cambiar una propiedad de renta tradicional a temporal.
3. Tratamiento contable de un ítem dañado y su reposición.
4. Días de anticipación para alertas de vencimiento.
5. Responsable autorizado para el cierre de mes.
