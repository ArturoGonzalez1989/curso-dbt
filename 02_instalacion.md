# 02 - Instalación y primer proyecto con dbt

## Instalación de dbt

Dependiendo del motor de base de datos que vayas a usar, dbt tiene adaptadores específicos. Aquí te mostramos cómo instalar dbt Core con el adaptador para **Snowflake**, uno de los más utilizados en entornos empresariales.

### Requisitos previos

- Python 3.7 o superior
- pip
- git

### Instalación usando pip

```bash
# Crear un entorno virtual (opcional pero recomendado)
python -m venv dbt-env
source dbt-env/bin/activate  # En Windows: dbt-env\Scripts\activate

# Instalar dbt con adaptador para Snowflake
pip install dbt-core dbt-snowflake
```

Puedes consultar otros adaptadores disponibles aquí: https://docs.getdbt.com/docs/available-adapters

---

## Verificar instalación

Una vez instalado, puedes verificar que todo esté correcto:

```bash
dbt --version
```

Deberías ver algo similar a:

```
dbt Core: 1.x.x
Plugins:
  - snowflake: 1.x.x
```

---

## Crear un nuevo proyecto dbt

Para crear tu primer proyecto dbt, usa el siguiente comando:

```bash
dbt init mi_proyecto_dbt
```

Este comando creará una estructura de carpetas como esta:

```
mi_proyecto_dbt/
├── dbt_project.yml       # Archivo principal de configuración del proyecto
├── models/               # Carpeta donde se definen los modelos (transformaciones SQL)
│   └── example/          # Modelos de ejemplo
├── macros/               # Carpeta para funciones personalizadas en Jinja
├── tests/                # Pruebas personalizadas (opcional)
├── snapshots/            # Para versionado de datos (opcional)
└── target/               # Carpeta generada con los resultados de ejecución
```

---

## Configurar la conexión a Snowflake

Edita el archivo `profiles.yml`, que se encuentra por defecto en:

- **Linux/Mac**: `~/.dbt/profiles.yml`
- **Windows**: `C:\Users\<usuario>\.dbt\profiles.yml`

Ejemplo de configuración para Snowflake:

```yaml
mi_proyecto_dbt:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: tu_cuenta.snowflakecomputing.com
      user: tu_usuario
      password: tu_contraseña
      role: TU_ROL
      database: TU_BASE_DATOS
      warehouse: TU_WAREHOUSE
      schema: analytics
      threads: 1
      client_session_keep_alive: false
```

> Puedes usar `dbt debug` para comprobar si la conexión está funcionando correctamente.

---

En el siguiente módulo comenzaremos a crear nuestros primeros modelos en SQL usando dbt -> 
