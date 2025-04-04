# Macros y Jinja en DBT

DBT permite utilizar la sintaxis de Jinja para dinamizar el código SQL y reutilizar lógica compleja mediante macros. En este módulo aprenderás a crear y utilizar macros personalizadas en tus modelos.

## ¿Qué es Jinja?

Jinja es un lenguaje de plantillas que permite insertar lógica como variables, condicionales y bucles dentro del código SQL.

DBT lo utiliza para:
- Referenciar modelos (`{{ ref('nombre_modelo') }}`).
- Aplicar configuraciones (`{{ config(...) }}`).
- Crear bloques condicionales y reutilizables.

## ¿Qué es una macro?

Una macro es una función escrita en Jinja que puedes reutilizar en múltiples modelos o pruebas. Se almacenan en archivos `.sql` dentro del directorio `macros/`.

### Ejemplo básico de macro

Archivo: `macros/formatear_fecha.sql`

```sql
{% macro formatear_fecha(campo) %}
  date_trunc('day', {{ campo }})
{% endmacro %}
```

Uso en un modelo:

```sql
select
  id,
  {{ formatear_fecha('fecha_registro') }} as fecha
from {{ ref('usuarios') }}
```

## Variables y lógica con Jinja

Puedes usar estructuras de control directamente dentro del modelo:

```sql
{% set columnas = ['id', 'nombre', 'email'] %}

select
  {% for col in columnas -%}
    {{ col }}{% if not loop.last %}, {% endif %}
  {% endfor %}
from {{ ref('clientes') }}
```

También puedes usar condicionales:

```sql
{% if target.name == 'dev' %}
  -- lógica para entorno de desarrollo
{% else %}
  -- lógica para producción
{% endif %}
```

## Uso de macros del ecosistema

DBT incluye macros útiles por defecto, y puedes instalar paquetes externos (como `dbt_utils`) para acceder a macros ya preparadas, por ejemplo:

```sql
select *
from {{ dbt_utils.union_relations(relations=[ref('tabla1'), ref('tabla2')]) }}
```

## Buenas prácticas

- Guarda las macros en la carpeta `macros/` y nómbralas de forma clara.
- Usa macros para evitar duplicación de código.
- Documenta tus macros igual que los modelos.
- Aprovecha variables, bucles y condicionales para mantener tu SQL limpio y DRY (Don't Repeat Yourself).

## Ejercicio práctico

1. Crea una macro personalizada que transforme una fecha en un formato específico.
2. Usa la macro en al menos un modelo.
3. Ejecuta `dbt run` y valida que el modelo se construye correctamente.

---

En el siguiente módulo veremos cómo trabajar con Sources (fuentes externas) y Seeds (archivos CSV).

