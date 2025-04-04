# Modelos y estructura de proyecto en DBT

En este módulo aprenderás los fundamentos sobre cómo se organizan los proyectos de dbt y cómo crear modelos básicos eficientemente.

## ¿Qué es un modelo en dbt?

Un modelo en dbt es simplemente un archivo SQL que contiene sentencias SELECT para transformar datos. DBT se encarga de compilar estos modelos en objetos reales dentro de tu base de datos (tablas o vistas).

### Tipos de modelos

DBT principalmente soporta tres tipos de modelos:

- **Views**: se ejecutan como vistas SQL en la base de datos.
- **Tables**: se almacenan físicamente en la base de datos.
- **Incremental**: permite actualizar tablas mediante la incorporación de nuevas filas basadas en lógica incremental.

## Estructura de un proyecto dbt

Un proyecto típico en dbt tiene una estructura definida para organizar modelos y código. Esta estructura normalmente incluye:

```
my-dbt-project/
├── dbt_project.yml
├── models/
│   ├── staging/
│   ├── intermediate/
│   └── marts/
├── seeds/
├── tests/
└── macros/
```

### Explicación básica de la estructura:

- **dbt_project.yml**: Archivo de configuración global del proyecto.
- **models**: Donde se almacenan todos tus modelos.
  - **staging**: Modelos de carga inicial desde fuentes (generalmente vistas).
  - **intermediate**: Modelos intermedios para transformaciones de datos complejas.
  - **marts**: Modelos finales preparados para análisis o reportes.
- **seeds**: Datos estáticos que quieres cargar en la base de datos.
- **tests**: Contiene pruebas para validar la calidad y consistencia de los datos.
- **macros**: Macros personalizadas y código reutilizable usando Jinja.

## Creación de tu primer modelo

1. Dentro de la carpeta `models`, crea un archivo llamado `primer_modelo.sql`.
2. Añade una sentencia SQL sencilla para comenzar:

```sql
select
  id,
  nombre,
  fecha_registro
from
  fuente_usuarios
```

3. Ejecuta tu modelo desde la terminal con:

```bash
dbt run
```

Esto compilará y ejecutará tu modelo en la base de datos.

## Dependencias entre modelos

Una característica clave de dbt es la definición explícita de dependencias entre modelos usando la función `ref()`.

Ejemplo de dependencia:

```sql
-- staging/stg_usuarios.sql
select id, nombre from fuente_usuarios;
```

```sql
-- intermediate/int_usuarios_activos.sql
select *
from {{ ref('stg_usuarios') }}
where activo = true;
```

DBT entiende automáticamente que `int_usuarios_activos` depende de `stg_usuarios` y ejecutará los modelos en el orden correcto.

## Compilar y ver SQL generado

Para revisar el SQL compilado de tu modelo, puedes ejecutar:

```bash
dbt compile
```

Esto generará un archivo en la carpeta `target/compiled` con el SQL exacto que se ejecutará en tu base de datos.

## Buenas prácticas al crear modelos

- Usa nombres claros y descriptivos.
- Separa modelos por niveles de abstracción (staging, intermedios, marts).
- Mantén tus modelos simples y fáciles de entender.
- Documenta cada modelo adecuadamente.

## Ejercicio práctico

- Crea al menos tres modelos básicos en tu proyecto dbt (`staging`, `intermediate` y `marts`).
- Usa correctamente la función `ref()` para gestionar dependencias.
- Ejecuta los modelos y revisa el SQL generado.

---

En el siguiente módulo veremos cómo gestionar diferentes formas de almacenamiento usando Materializations.

