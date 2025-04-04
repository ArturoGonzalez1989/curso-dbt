# Despliegue y CI/CD en DBT

Una vez que tu proyecto dbt está listo y probado localmente, es importante automatizar su ejecución y despliegue. En este módulo aprenderás cómo configurar un flujo de CI/CD (Integración Continua y Despliegue Continuo) para tu proyecto dbt.

## ¿Qué es CI/CD en el contexto de dbt?

- **CI (Integración Continua)**: Automatiza la ejecución de pruebas, compilación de modelos y validaciones cada vez que se realiza un cambio en el proyecto.
- **CD (Despliegue Continuo)**: Permite programar o ejecutar automáticamente el proyecto en entornos productivos tras una validación exitosa.

## Opciones para automatizar dbt

### 1. dbt Cloud

- Plataforma oficial de dbt.
- Permite programar ejecuciones, revisar cambios y visualizar la documentación.
- Integra fácilmente con GitHub, GitLab o Bitbucket.

Pasos básicos:
1. Conecta tu repositorio.
2. Configura un entorno (dev / prod).
3. Define jobs con comandos como `dbt run`, `dbt test`, etc.

### 2. CI/CD con GitHub Actions

Puedes usar GitHub Actions para automatizar tareas con cada `push` o `pull request`.

Ejemplo básico de flujo:

Archivo: `.github/workflows/dbt_ci.yml`

```yaml
name: dbt CI

on:
  pull_request:
    branches: [ main ]

jobs:
  dbt-check:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.10'

      - name: Install dependencies
        run: |
          pip install dbt-core dbt-bigquery  # o dbt-snowflake, etc.

      - name: Run dbt commands
        run: |
          dbt deps
          dbt seed
          dbt run
          dbt test
```

### 3. Orquestadores como Airflow, Prefect o Dagster

- Permiten incluir dbt como parte de un flujo de trabajo más amplio.
- Ejecutan comandos como `dbt run` o `dbt test` mediante scripts o hooks.

## Buenas prácticas

- Asegúrate de que todos los modelos y tests pasen antes de desplegar a producción.
- Usa entornos separados para desarrollo y producción.
- Mantén tu proyecto dbt bajo control de versiones (Git).
- Automatiza la generación de documentación (`dbt docs generate`).

## Ejercicio práctico

1. Crea un flujo de CI con GitHub Actions o en dbt Cloud.
2. Configura los comandos `dbt run` y `dbt test` al realizar un push o PR.
3. Verifica que los tests se ejecutan automáticamente con cada cambio.

---

¡Felicidades! Has completado todos los módulos del curso de dbt. Ahora estás listo para construir pipelines de datos robustos, documentados y automatizados con dbt.

