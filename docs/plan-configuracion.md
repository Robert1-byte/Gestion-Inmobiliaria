# Plan de configuración y administración de GitHub

## Líneas base y ramas

- `main`: rama protegida; único destino de PRs.
- `feature/I1-<modulo>`, `fix/I1-<id>`, `docs/<artefacto>`, `infra/<tema>`.
- `release/I1-1.0.0`: estabilización del incremento 1.
- Tags: `SRS-v1.0`, `MER-v1.0`, `I1-v1.0.0`, `I2-v1.0.0`.

## Protección de `main`

Configurar con `gh` (o equivalente en GitHub Web):

```bash
gh api -X PUT repos/Robert1-byte/Gestion-Inmobiliaria/branches/main/protection \
  -f required_status_checks.strict=true \
  -f required_status_checks.contexts[]='backend-ci / build' \
  -f enforce_admins=false \
  -f required_pull_request_reviews.required_approving_review_count=1 \
  -f required_pull_request_reviews.dismiss_stale_reviews=true \
  -F restrictions=null \
  -f allow_force_pushes=false \
  -f allow_deletions=false
```

## Milestones

- `Actividad 1` — 18/11/2026.
- `I1 — Entrega parcial` — 18/12/2026.
- `I2 — Entrega final` — 01–04/02/2027.

## Etiquetas

`incremento-1`, `incremento-2`, `modulo:propiedades`, `modulo:contratos`, `modulo:inventario`, `modulo:finanzas`, `modulo:reportes`, `tipo:historia`, `tipo:bug`, `tipo:tarea`, `prioridad:alta`, `evidencia`, `documentacion`.

```bash
for label in "incremento-1:1B7F5A" "incremento-2:0E639C" "modulo:propiedades:5319E7" "modulo:contratos:5319E7" "modulo:inventario:5319E7" "modulo:finanzas:5319E7" "modulo:reportes:5319E7" "tipo:historia:C2E0C6" "tipo:bug:D73A4A" "tipo:tarea:0075CA" "prioridad:alta:B60205" "evidencia:F9D0C4" "documentacion:0052CC"; do
  gh label create "${label%%:*}" --color "${label##*:}" --force
done
```

## Integración continua

`.github/workflows/backend-ci.yml` ejecuta `mvn -B verify` en JDK 21 y publica los reportes de pruebas. El check debe aprobarse antes de fusionar.

## Seguridad y evidencia

- `.env.example` es la única plantilla versionada; `.env` está ignorado.
- Los PDF de contratos se almacenan fuera del repositorio y fuera de PostgreSQL.
- Cada entrega se publica como GitHub Release con changelog, dossier y evidencias.

## Correspondencia ISO/IEC/IEEE 12207:2026

| Configuración | Proceso |
|---|---|
| Acta, autorización y responsables | 6.2.3 |
| Ramas, tags, PR y releases | 6.3.5 |
| Documentos en `docs/` y `evidencias/` | 6.3.6 |
| Historias, RF y RNF | 6.4.2 y 6.4.3 |
