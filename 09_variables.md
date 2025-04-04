# Variables y configuración en DBT

DBT permite personalizar modelos y macros mediante el uso de variables (`vars`) y configuraciones (`config`). Este módulo te enseñará a utilizarlas para hacer tu proyecto más dinámico y flexible.

## ¿Qué es una variable en dbt?

Una variable es un valor definido en tu archivo `dbt_project.yml` o en tiempo de ejecución, que puedes usar en tus modelos o macros.

### Definir variables

En `dbt_project.yml`:

```yaml
vars:
  limite_registros: 1000
  entorno: "dev"
```

También puedes pasar variables desde la línea de comandos:

```bash
dbt run --vars '{"limite_registros": 1000}'
```

### Usar variables en un modelo

```sql
{% set limite = var('limite_registros', 100) %}

select *
from {{ ref('usuarios') }}
limit {{ limite }}
```

Si la variable no está definida, se usa el valor por defecto (`100` en este caso).

## Configuraciones (`config`)

Las configuraciones definen el comportamiento de un modelo o seed. Se pueden aplicar de dos maneras:

### 1. En línea dentro del modelo

```sql
{{ config(materialized='table', schema='analytics') }}
```

### 2. En el archivo `dbt_project.yml`

```yaml
models:
  my_project:
    staging:
      +materialized: view
    marts:
      +materialized: table
```

## Ejemplo completo

```sql
{{ config(
    materialized='incremental',
    unique_key='id'
) }}

{% set dias = var('dias_historico', 30) %}

select *
from {{ ref('eventos') }}
where fecha_evento >= current_date - interval '{{ dias }} days'
{% if is_incremental() %}
  and fecha_evento > (select max(fecha_evento) from {{ this }})
{% endif %}
```

## Buenas prácticas

- Usa variables para adaptar el comportamiento del modelo según el entorno.
- Establece valores por defecto con `var('nombre', valor_defecto)`.
- Centraliza configuraciones comunes en `dbt_project.yml`.
- Usa `config()` en modelos específicos cuando se necesite un comportamiento distinto.

## Ejercicio práctico

1. Define una variable `limite_registros` en tu archivo `dbt_project.yml`.
2. Úsala en un modelo para limitar los resultados.
3. Cambia su valor desde la línea de comandos y observa cómo cambia el comportamiento del modelo.

---

En el próximo módulo aprenderás a desplegar tu proyecto y configurar un flujo CI/CD con dbt.

