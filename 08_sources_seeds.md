# Sources y Seeds en DBT

En este módulo aprenderás cómo manejar fuentes de datos externas (`sources`) y cómo cargar datos estáticos desde archivos CSV (`seeds`). Estas funcionalidades son esenciales para integrar y validar tus datos de entrada.

## ¿Qué es un Source en dbt?

Un `source` permite registrar y documentar las tablas externas que no son gestionadas por dbt (por ejemplo, tablas que ya existen en tu data warehouse y se usan como entrada).

### Declarar un source

En un archivo `schema.yml`, puedes definir un source así:

```yaml
version: 2

sources:
  - name: datos_externos
    description: "Tablas cargadas por otros sistemas."
    database: mi_base_de_datos
    schema: raw
    tables:
      - name: usuarios
        description: "Usuarios registrados en la aplicación."
```

### Usar un source en un modelo

```sql
select *
from {{ source('datos_externos', 'usuarios') }}
```

Esto asegura que dbt sepa que estás utilizando una tabla externa y podrá mapear relaciones en la documentación.

## ¿Qué es un Seed en dbt?

Un `seed` es un archivo CSV que forma parte del proyecto dbt y que puede cargarse en la base de datos como una tabla. Ideal para datos estáticos, listas de referencia o configuraciones.

### Pasos para usar seeds

1. Coloca tus archivos CSV en la carpeta `seeds/`.
2. Ejecuta el comando:

```bash
dbt seed
```

Esto cargará los datos en la base de datos.

### Configuración opcional en `dbt_project.yml`

```yaml
seeds:
  my_project:
    +schema: referencia
    +quote_columns: false
```

### Usar un seed en un modelo

```sql
select * from {{ ref('mi_seed') }}
```

## Diferencias clave entre sources y seeds

| Característica       | Sources                           | Seeds                         |
|----------------------|------------------------------------|-------------------------------|
| Origen               | Externo al proyecto dbt            | Interno (archivos CSV locales) |
| Gestión              | dbt no los crea ni modifica        | dbt los carga como tablas     |
| Uso común            | Datos de entrada reales            | Tablas de referencia estáticas |

## Buenas prácticas

- Documenta bien tus sources para mayor trazabilidad.
- Usa seeds para datos que cambian raramente.
- No abuses de seeds para grandes volúmenes de datos.
- Revisa los permisos necesarios para cargar seeds.

## Ejercicio práctico

1. Declara un source en un archivo `schema.yml` y utilízalo en un modelo.
2. Añade un archivo CSV en `seeds/`, ejecútalo con `dbt seed` y crea un modelo que lo utilice.

---

En el próximo módulo veremos cómo trabajar con variables y configuración avanzada en dbt.

