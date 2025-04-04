# Tests en DBT

Uno de los pilares fundamentales de dbt es asegurar la calidad de los datos mediante tests automáticos. En este módulo aprenderás a crear y aplicar diferentes tipos de tests.

## ¿Qué es un test en dbt?

Un test en dbt es una validación que se ejecuta sobre tus datos para comprobar su consistencia, integridad o exactitud. Los tests permiten detectar errores temprano en el pipeline de datos.

## Tipos de tests en dbt

### 1. Tests genéricos (built-in)

Estos son los más comunes y fáciles de aplicar. Algunos ejemplos:

- `unique`: verifica que los valores de una columna sean únicos.
- `not_null`: verifica que no haya valores nulos.
- `accepted_values`: valida que una columna solo contenga ciertos valores.
- `relationships`: asegura que existe una clave foránea entre tablas.

#### Ejemplo:

```yaml
version: 2

models:
  - name: clientes
    columns:
      - name: id
        tests:
          - unique
          - not_null
      - name: estado
        tests:
          - accepted_values:
              values: ['activo', 'inactivo']
```

### 2. Tests personalizados (singular tests)

Son modelos SQL que tú defines para validar condiciones específicas.

#### Ejemplo de test personalizado:

Archivo: `tests/no_clientes_sin_email.sql`

```sql
select *
from {{ ref('clientes') }}
where email is null
```

Si este test devuelve alguna fila, dbt lo marcará como fallido.

## Cómo ejecutar los tests

Puedes ejecutar todos los tests del proyecto con:

```bash
dbt test
```

También puedes ejecutar tests para un modelo específico:

```bash
dbt test --select clientes
```

## Revisión de resultados

Los resultados se mostrarán en la terminal. Los tests fallidos mostrarán detalles de qué filas no cumplen las condiciones.

Puedes revisar los logs o utilizar herramientas como `dbt Cloud` o `dbt docs` para ver el historial de tests.

## Buenas prácticas

- Aplica tests `unique` y `not_null` a todas tus claves primarias.
- Usa `relationships` para simular integridad referencial.
- Documenta tus tests junto con tus modelos.
- Agrega tests personalizados para validar reglas de negocio importantes.

## Ejercicio práctico

1. Define al menos un test `unique` y `not_null` en un modelo existente.
2. Crea un test personalizado que valide una condición importante de tu negocio.
3. Ejecuta `dbt test` y analiza los resultados.

---

En el próximo módulo exploraremos cómo generar documentación interactiva de tus modelos con `dbt docs`.

