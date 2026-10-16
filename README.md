<!-- # Dashboard-de-Tramite-Documentario-y-Flujo-de-Recepción-Enaco
Dashboard interactivo en Power BI para el análisis, seguimiento y control del flujo de recepción documental en ENACO S.A.


---

## 📌 1. Visión General del Proyecto

Este proyecto implementa una solución integral de **Business Intelligence (BI)** orientada al análisis, control y optimización del **Trámite Documentario y Flujo de Recepción** en la **Empresa Nacional de la Coca S.A. (ENACO S.A.)**.

A través del procesamiento y modelado de datos transaccionales de recepción documental, la solución permite monitorear el ciclo de vida completo de cada expediente, evaluar la eficiencia operativa de los analistas, auditar los canales de entrada (plataformas digitales y físicas) y medir el cumplimiento de los tiempos de atención (SLA).

---

## 🛠️ 2. Arquitectura de Datos y Variables Analizadas

El modelo procesa registros transaccionales detallados con la siguiente estructura analítica de variables:

| Variable | Tipo de Dato | Descripción y Uso Analítico |
| :--- | :--- | :--- |
| `CODIGO_DE_DOCUMENTO` | Cadena / Código | Identificador único del documento. |
| `FECHA_DE_INGRESO` | Fecha (`DD-MM-YYYY`) | Fecha oficial de registro / autorización de ingreso del documento. |
| `HORA_DE_INGRESO` | Hora (`HH:MM:SS`) | Hora exactas de registro. |
| `FECHA_DE_ATENCION` | Fecha (`DD-MM-YYYY`) | Fecha en la que el analista completó o atendió el documento. |
| `HORA_DE_ATENCION` | Hora (`HH:MM:SS`) | Hora exacta de resolución o despacho del trámite. |
| `DIAS_DE_ATENCION` | Número Entero | Numero de dias en la que se demoro el documento en ser atendido. |
| `DIM_TIPO_CANAL` | Dimensión / Canal | Canal de ingreso: `FACILITA`, `FISICO`, `CORREO ELECTRONICO`, `DIRECTA`, `PIDE/SIED`. |
| `DIM_ESTADO` | Dimensión / Estado | Estado en el que se encuentra el documento :`ATENDIDO`, `PROCESO` . |
| `DIM_ATENCION` | Dimensión / Estado | Si el documento fue atendido el mismo día:`SI`, `NO` . |

---

## 🔄 3. Flujo de Trabajo Profesional Paso a Paso (Workflow)

Para lograr un producto analítico listo para producción y tomar decisiones informadas, se ejecutó la siguiente metodología de trabajo:



### Paso 1: Extracción, Transformación y Limpieza de Datos (ETL)
* **Ingesta de Datos:** Carga de registros desde el sistema transaccional de trámite documentario.
* **Limpieza y Estandarización:**
  * Tratamiento de valores nulos o caracteres nulos (`\N`) en fechas y horas de atención (`fecha_atencion`, `hora_atencion`).
  * Normalización de cadenas de texto y eliminación de espacios redundantes en nombres de usuario (`pers_login`) y canales (`trec_nombre`).
  * Parseo y conversión estricta de tipos de datos a formato `Date`, `Time` e `Integer`.

### Paso 2: Creación de Variables Derivadas y Métricas de Tiempo
* **Cálculo de Lead Time (Tiempo de Atención):** Generación de columnas calculadas para medir la diferencia exacta en días y horas entre la recepción (`docu_fauto` + `docu_hauto`) y la atención (`fecha_atencion` + `hora_atencion`).
* **Categorización de Canales:** Agrupación de medios de entrada en Canales Digitales (`FACILITA`, `CORREO ELECTRONICO`, `PIDE/SIED`) vs. Canales Presenciales (`FISICO`, `DIRECTA`).

### Paso 3: Modelado de Datos (Star Schema)
* **Tabla de Hechos (`Fact_TramiteDocumentario`):** Almacena cada transacción de recepción y atención documental.
* **Tablas de Dimensiones (`Dim_Calendario`, `Dim_Canal`, `Dim_Usuario`):**
  * Creación de una **Dimensión Calendario** personalizada con jerarquías completas (Año, Mes, Trimestre, Día de la semana, Día laborable).
  * Establecimiento de relaciones unidireccionales `1 a Muchos` con filtro en una sola dirección para optimizar el rendimiento del motor VertiPaq.

### Paso 4: Desarrollo de Métricas y Cálculos DAX
Implementación de medidas analíticas dinámicas, entre las que destacan:
* **Total de Documentos Ingresados:** `COUNTROWS(Fact_TramiteDocumentario)`
* **Documentos Atendidos vs. En Proceso:** Filtrado dinámico según estado y presencia de fecha de atención.
* **Tiempo Promedio de Atención (Días):** `AVERAGEX` sobre la diferencia de fechas entre registro y despacho.
* **Distribución de Carga por Analista:** Porcentaje del volumen total atendido por usuario (`pers_login`).
* **Análisis Time-Intelligence:** Comparativos intermensuales (MoM) e interanuales (YoY) de recepción documental.

### Paso 5: Diseño de Interfaz y Experiencia de Usuario (UI/UX)
* **Maquetación Visual:** Diseño estructurado por capas con tarjetas de KPIs en la parte superior, gráficos de barras/líneas en el cuerpo y tablas de detalle operativo en la base.
* **Panel de Filtros Interactivos (Slicers):** Segmentación fluida por rango de fechas, canal de ingreso (`trec_nombre`), estado del trámite y analista asignado.
* **Navegación e Interactivad:** Configuración de Tooltips personalizados y filtrado cruzado para facilitar la exploración de datos a nivel macro y micro.

---

## 🚀 4. Resultados y Valor de Negocio Impulsado

Con la implementación de este Dashboard analítico, la entidad logra:

1. **Gobernanza y Trazabilidad:** Monitoreo transparente sobre el estado y la acción tomada (`acci_descripc`) en cada expediente.
2. **Identificación de Cuellos de Botella:** Detección de demoras operativas por analista (`pers_login`) o por canal de recepción.
3. **Estrategia Digital (Transformación Digital):** Medición de la adopción de canales digitales estatales (`PIDE/SIED`, `FACILITA`) frente al trámite físico tradicional.
4. **Reducción de Tiempos de Respuesta (SLA):** Optimización del tiempo promedio de procesamiento para brindar respuestas oportunas al usuario o entidad remitente.

---

## 📂 5. Estructura del Repositorio

```text
├── assets/                  # Capturas de pantalla e imágenes del Dashboard
├── data/                    # Set de datos transaccionales anonimizados (.csv)
├── src/                     # Archivo fuente de Power BI (.pbix)
└── README.md                # Documentación técnica del proyecto

``` -->



# 📊 ENACO S.A. — Trámite Documentario & Flujo de Recepción Analytics

![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Power Query](https://img.shields.io/badge/Power_Query-ETL-green?style=for-the-badge)
![DAX](https://img.shields.io/badge/DAX-Advanced-blue?style=for-the-badge)
![Data Analytics](https://img.shields.io/badge/Data_Analytics-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)

---

## 📌 1. Descripción General del Proyecto

Este repositorio presenta una solución integral de **Business Intelligence (BI)** desarrollada para la **Empresa Nacional de la Coca S.A. (ENACO)**. El sistema centraliza y analiza el flujo completo de documentos recibidos en la institución, transformando registros transaccionales complejos en un **Dashboard Interactivo en Power BI**.

El objetivo principal es responder a preguntas clave de negocio y auditoría operativa: *¿Qué canales de recepción generan mayor volumen?, ¿Cuál es el tiempo real de respuesta (Lead Time) por analista?, y ¿Cómo se distribuye la carga operativa de trámites a lo largo del tiempo?*

---

## 🎯 2. Preguntas Clave de Negocio (Business Questions)

El dashboard fue diseñado específicamente para dar respuesta a las siguientes incógnitas operativas y gerenciales:
* **Volumen por Canal:** ¿Cuáles son los canales de recepción (`trec_nombre`) más utilizados por los ciudadanos y entidades (ej. *PIDE/SIED, FACILITA, Correo Electrónico, Físico, Directa*)?
* **Tiempos de Respuesta (SLA):** ¿Cuánto tiempo transcurre en promedio entre la autorización/registro del documento (`docu_fauto`) y su atención final (`fecha_atencion`)?
* **Carga Laboral:** ¿Cómo se distribuye el volumen de trámites atendidos entre los diferentes operadores (`pers_login`)?
* **Seguimiento de Acciones:** ¿Qué tipos de derivaciones o gestiones internas (`acci_descripc`) predominan en la resolución de los expedientes?

---

## 🛠️ 3. Estructura de Datos y Variables Analizadas

El modelo procesa datos relacionales estructurados bajo las siguientes variables clave:

| Campo / Columna | Tipo | Descripción Analítica |
| :--- | :--- | :--- |
| `docu_cod` | Identificador | Código único del expediente o trámite documentario. |
| `docu_fauto` / `docu_hauto` | Fecha y Hora | Timestamp exacto de la autorización y recepción oficial del documento. |
| `fecha_atencion` / `hora_atencion` | Fecha y Hora | Timestamp exacto de la resolución y despacho del trámite. |
| `pers_login` | Categoria | Identificador del analista o servidor responsable de la atención. |
| `trec_nombre` | Dimensión | Canal de entrada o medio por el cual ingresó el documento. |
| `acci_descripc` | Texto / Log | Descripción detallada de la acción realizada (derivaciones, correos, etc.). |
| `documentos_proceso` | Estado | Estado actual del trámite (ej. *En proceso*, *Atendido*). |

---

## 🔄 4. Flujo de Trabajo y Metodología (End-to-End Workflow)

1. **Extracción y Limpieza de Datos (ETL):** 
   * Ingesta de registros transaccionales.
   * Tratamiento de nulos, estandarización de cadenas de texto y corrección de marcas temporales (`\N`).
2. **Modelado Dimensional (Star Schema):** 
   * Creación de la tabla de hechos central y dimensiones satélites (*Dimensión de Tiempo*, *Canales*, *Usuarios*).
3. **Desarrollo de Medidas DAX:** 
   * Construcción de métricas dinámicas para conteo de expedientes, cálculo de brechas temporales (Lead Time) y análisis de rendimiento.
4. **Diseño de Visuales y UI/UX:** 
   * Estructuración jerárquica con tarjetas de KPIs principales, gráficos de tendencias temporales, segmentadores interactivos y tablas de detalle operativo.

---

## 📈 5. Características Principales del Dashboard

* **KPI Cards (Tarjetas Ejecutivas):** Visualización inmediata del total de documentos ingresados, trámites en proceso y tiempos promedio de atención.
* **Análisis Multicanal:** Gráficos comparativos del comportamiento de entrada digital vs. presencial.
* **Filtros Dinámicos (Slicers):** Segmentación por rangos de fecha, analista responsable, canal de recepción y estado del trámite.
* **Time-Intelligence:** Seguimiento de la evolución de la carga documental por mes, semana y día.

---

## 🚀 6. Valor de Negocio e Impacto

* **Reducción de Cuellos de Botella:** Identificación temprana de áreas o canales saturados.
* **Transparencia y Auditoría:** Trazabilidad absoluta sobre qué usuario gestionó un expediente y qué acciones se tomaron.
* **Mejora del SLA Institucional:** Optimización de los tiempos de respuesta al ciudadano y a las entidades del Estado.

---

## 📂 7. Estructura del Repositorio

```text
├── assets/                  # Capturas de pantalla e interfaz visual del Dashboard
├── data/                    # Muestras de datos anonimizados (.csv)
├── src/                     # Archivo fuente de Power BI (.pbix)
└── README.md                # Documentación técnica del proyecto