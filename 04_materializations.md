# Materializations en DBT

Las *materializations* en dbt controlan cómo se materializa un modelo en la base de datos. Esto es crucial para optimizar rendimiento, gestionar el coste de almacenamiento y definir la lógica de actualización de tus datos.

## ¿Qué es una materialization?

Es la forma en la que dbt construye un modelo en la base de datos. Por ejemplo, una materialization puede indicar si un modelo debe ser creado como una **vista**, una **tabla** o una tabla **incremental**.

## Tipos de materializations estándar

DBT incluye varios tipos de materializations por defecto:

### 1. `view`

- Crea una vista en la base de datos.
- Rápida de construir, pero se recalcula en cada consulta.
- No ocupa almacenamiento adicional.

```yaml
-- models/stg_clientes.sql
{{ config(materialized='view') }}
```

### 2. `table`

- Crea una tabla física.
- Buena para modelos complejos o que se consultan frecuentemente.
- Se recrea por completo cada vez que se ejecuta el modelo.

```yaml
-- models/dim_productos.sql
{{ config(materialized='table') }}
```

### 3. `incremental`

- Permite construir tablas que se actualizan con nuevas filas.
- Útil para grandes volúmenes de datos donde sólo se requiere añadir información nueva.
- Requiere una condición incremental para identificar nuevos datos.

```sql
{{
  config(
    materialized='incremental',
    unique_key='id'
  )
}}

select * from fuente_transacciones
{% if is_incremental() %}
  where fecha > (select max(fecha) from {{ this }})
{% endif %}
```

### 4. `ephemeral`

- No se crea ningún objeto físico en la base de datos.
- Se incrusta el SQL del modelo directamente en otros modelos.
- Útil para subconsultas reutilizables o cálculos intermedios pequeños.

```yaml
-- models/calc_margen.sql
{{ config(materialized='ephemeral') }}
```

## Cómo definir una materialization

Se define con la función `config()` al principio del modelo o en el archivo `dbt_project.yml`.

### En el modelo directamente

```sql
{{ config(materialized='table') }}
select * from otra_tabla
```

### En `dbt_project.yml`

```yaml
models:
  my_project:
    staging:
      +materialized: view
    marts:
      +materialized: table
```

## Consideraciones al elegir materialization

- Usa `view` para modelos simples y livianos.
- Usa `table` para modelos pesados o consultados frecuentemente.
- Usa `incremental` para datos que crecen en el tiempo.
- Usa `ephemeral` para lógica que se puede incrustar en otros modelos sin coste adicional.

## Buenas prácticas

- Define las materializations explícitamente, especialmente en modelos críticos.
- Usa `ephemeral` solo para lógica ligera o cálculos reutilizables.
- Para `incremental`, asegúrate de que la condición `is_incremental()` esté correctamente implementada.

## Ejercicio práctico

1. Crea cuatro modelos usando cada una de las materializations descritas.
2. Ejecuta `dbt run` y verifica cómo se construyen en la base de datos.
3. Explora cómo se comportan al hacer cambios y volver a ejecutar.

---

En el siguiente módulo aprenderás a validar la calidad de tus datos usando Tests en dbt.