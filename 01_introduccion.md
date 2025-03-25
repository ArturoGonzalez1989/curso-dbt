# 01 - Introducción a dbt (Data Build Tool)

## ¿Qué es dbt?

**dbt (data build tool)** es una herramienta de transformación de datos pensada para analistas e ingenieros de datos que trabajan con modelos analíticos en SQL. Permite transformar datos de forma modular, versionada y reutilizable.

Con dbt puedes:

- Escribir transformaciones de datos como SELECTs.
- Organizar y reutilizar SQL con estructuras claras.
- Versionar tu proyecto como código (Git).
- Probar y documentar los modelos fácilmente.
- Automatizar pipelines de datos con CI/CD.

## Versiones de dbt: Core vs Cloud

Existen dos versiones principales de dbt:

### dbt Core

- Es la versión **open source**.
- Se ejecuta desde línea de comandos (CLI).
- Se instala localmente o en servidores propios.
- Requiere configuración manual (entorno, conexión al data warehouse, CI/CD, etc.).
- Ideal para equipos técnicos que buscan control total y flexibilidad.

### dbt Cloud

- Versión **gestionada por dbt Labs**.
- Incluye una interfaz web para ejecutar, programar y documentar modelos.
- Integraciones listas para usar (GitHub, CI/CD, alertas, etc.).
- Ideal para equipos mixtos (analistas e ingenieros) o empresas que prefieren no gestionar infraestructura.

Ambas versiones comparten la misma base: los modelos y proyectos creados en dbt Core funcionan en dbt Cloud y viceversa.

## ¿Por qué usar dbt?

- **Modularidad**: divide tus transformaciones en modelos reutilizables.
- **Transparencia**: cada paso en la transformación está definido como SQL legible.
- **Versionado**: dbt se integra perfectamente con Git.
- **Documentación automática**: genera documentación navegable del proyecto.
- **Pruebas de datos**: valida calidad y consistencia de tus datos con tests integrados.

## Cómo encaja dbt en el stack moderno de datos

dbt se centra exclusivamente en la **transformación** de datos (la "T" de ELT):

```
[Origen de datos] --(Extract & Load)--> [Data Warehouse] --(Transformación con dbt)--> [Datos analíticos]
```

Funciona sobre almacenes de datos como:

- Snowflake
- BigQuery
- Redshift
- Databricks
- Postgres (ideal para pruebas locales)

## Requisitos previos para usar dbt

Antes de comenzar con dbt, es ideal tener:

- Conocimientos básicos de SQL.
- Familiaridad con Git y línea de comandos.
- Acceso a un almacén de datos (como Snowflake o BigQuery).

## ¿Qué aprenderás en este curso?

- Instalar y configurar un proyecto dbt.
- Crear modelos de transformación en SQL.
- Implementar tests de calidad de datos.
- Documentar tus modelos y generar documentación navegable.
- Automatizar tareas con macros y variables.
- Integrar dbt en flujos de CI/CD para despliegues automáticos.

---

En el siguiente módulo aprenderás a instalar dbt y crear tu primer proyecto -> [Instalación y configuración](02_instalacion.md)