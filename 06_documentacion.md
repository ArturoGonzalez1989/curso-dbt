# Documentación con dbt docs

DBT permite documentar tus modelos, tests, fuentes y más, generando una interfaz visual interactiva para explorar tu proyecto de datos. En este módulo aprenderás cómo aprovechar `dbt docs` para generar documentación útil y navegable.

## ¿Qué es dbt docs?

`dbt docs` es una herramienta que crea documentación estática del proyecto, incluyendo:

- Descripciones de modelos y columnas.
- Relaciones entre modelos.
- Tests aplicados a cada modelo.
- Estadísticas de ejecución (si se usa `dbt Cloud` o `dbt artifacts`).

## Documentar modelos y columnas

Puedes añadir documentación en el archivo `schema.yml` junto con los tests:

```yaml
version: 2

models:
  - name: clientes
    description: "Tabla de clientes activos e inactivos."
    columns:
      - name: id
        description: "Identificador único del cliente."
      - name: email
        description: "Correo electrónico principal."
```

También puedes documentar **sources**, **macros** y **seeds** de manera similar.

## Comandos principales

### 1. Generar la documentación

```bash
dbt docs generate
```

Este comando crea un sitio web estático en la carpeta `target/` con todos los detalles del proyecto.

### 2. Servir la documentación en local

```bash
dbt docs serve
```

Este comando lanza un servidor local en tu navegador para navegar la documentación.

## Explorar la documentación

La interfaz de `dbt docs` te permite:

- Ver el DAG (grafo de dependencias entre modelos).
- Hacer clic sobre modelos para ver descripción, SQL compilado, columnas, tests y relaciones.
- Navegar por sources, seeds, snapshots y más.

## Buenas prácticas de documentación

- Añade una descripción a cada modelo y columna.
- Documenta las fuentes (`sources`) para mostrar de dónde provienen los datos.
- Mantén la documentación actualizada junto con los cambios del modelo.
- Usa nombres descriptivos y consistentes para facilitar la navegación.

## Ejercicio práctico

1. Agrega descripciones a tres modelos y sus columnas en el archivo `schema.yml`.
2. Ejecuta `dbt docs generate` y `dbt docs serve`.
3. Explora la documentación generada desde el navegador.

---

En el siguiente módulo aprenderemos a extender la funcionalidad de dbt con Macros y Jinja.

