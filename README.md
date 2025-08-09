# Proyecto: Sistema Contable con Clasificación Automática mediante LLMs

## Descripción

Este proyecto consiste en un sistema contable básico que permita:

1. **Cargar datos contables** desde un archivo Excel con una estructura predefinida.
2. **Clasificar automáticamente las transacciones** utilizando un modelo de lenguaje (LLM) para asignarles tipo, categoría y cuenta contable.
3. **Editar y gestionar datos** mediante un CRUD sencillo para:

   * Corregir errores de clasificación del LLM.
   * Agregar nuevas transacciones.
   * Eliminar transacciones existentes.
4. **Filtrar por periodos contables** (mensual, trimestral, anual).
5. **Verificar balance contable** asegurando que el Debe y el Haber cuadren.
6. **Exportar resultados** en:

   * Formato **Excel**.
   * Formato **HTML** listo para impresión.
7. **Ejecutarse en contenedores Docker** para facilitar despliegue y uso.

---

## Requerimientos funcionales

### 1. Carga de datos

* El sistema debe permitir subir un archivo **Excel** con la siguiente estructura:

| Fecha      | Descripción                  | Monto  | Moneda |
| ---------- | ---------------------------- | ------ | ------ |
| 2025-08-01 | Venta de 5 camisetas         | 50.00  | USD    |
| 2025-08-01 | Compra de papel para oficina | 10.00  | USD    |
| 2025-08-02 | Pago de alquiler mensual     | 200.00 | USD    |

* Validar:

  * Formato de fecha correcto (`YYYY-MM-DD`).
  * Monto positivo.
  * Moneda permitida (ej: `USD`).
  * Columnas obligatorias presentes.

---

### 2. Clasificación automática con LLM

* Utilizar un **modelo de lenguaje** (ej. GPT-4o-mini, Claude Haiku o LLM local) para:

  * Determinar si la transacción es **Ingreso** o **Egreso**.
  * Asignar una **categoría** (Ventas, Compras, Servicios, etc.).
  * Asignar una **cuenta contable** basada en un catálogo de cuentas de El Salvador.
* Guardar los resultados en una base de datos o estructura en memoria.

---

### 3. Módulo CRUD de transacciones

* **Listar** todas las transacciones cargadas.
* **Editar** transacciones:

  * Corregir tipo, categoría y cuenta contable asignada por el LLM.
  * Modificar monto o fecha.
* **Agregar** nuevas transacciones manualmente.
* **Eliminar** transacciones existentes.

---

### 4. Filtrado por periodos contables

* Posibilidad de filtrar transacciones y reportes por:

  * Mes
  * Trimestre
  * Año
* Ejemplo: Filtrar todas las transacciones de **agosto 2025**.

---

### 5. Verificación de balance contable

* El sistema debe validar que:

  * La suma del **Debe** sea igual a la suma del **Haber**.
* Mostrar advertencia si los valores no cuadran.
* La validación debe suceder al presionar un boton 'Validar' en el menu del proyecto, debe indicar los errores.

---

### 6. Exportación de reportes

* **Excel**:

  * Exportar **Libro Diario** y **Libro Mayor** con formato y sumatorias.
* **HTML**:

  * Generar un reporte visual listo para imprimir desde el navegador.
  * Incluir encabezados, fechas, totales y formato de tabla contable.

---

## Requerimientos técnicos

### Lenguaje y frameworks sugeridos

* **Python**:

  * Backend: Flask o FastAPI.
  * Procesamiento de Excel: Pandas + openpyxl.
  * Exportación HTML: Jinja2 templates.
* **Alternativa Node.js**:

  * Backend: Express.js.
  * Procesamiento de Excel: SheetJS (xlsx).
  * Plantillas HTML: EJS o Handlebars.

### Integración con LLM

* API de OpenAI (`gpt-4o-mini` o `gpt-3.5-turbo`).
* Alternativa: AWS Bedrock (Claude Haiku).
* Parámetros:

  * `temperature = 0` para respuestas deterministas.
  * Respuesta en formato JSON.

### Base de datos

* Opciones:

  * SQLite (sencillo y portable).
  * Archivos JSON para almacenamiento básico.

---

## Contenedorización (Docker)

El proyecto debe poder ejecutarse con **Docker** usando un `docker-compose.yml` en la raíz, que levante:

* **Backend** (contenedor independiente).
* **Frontend** (contenedor independiente).

### Variables de entorno

El archivo `.env` en la raíz del proyecto debe contener:

```
PORT_BE=
PORT_FE=
```

El **Backend** debe leer estas variables para:
- Conectarse al proveedor de LLM.
- Configurar el puerto.
- Ubicar la base de datos.

---

## Control de versiones

- El repositorio de **GitHub** será creado por **uno de los estudiantes** y compartido con el resto del equipo.
- Se debe utilizar **Gitflow** para el manejo de ramas:
  - `main` → Producción.
  - `develop` → Desarrollo.
  - Ramas de feature → `feature/nombre`.
  - Ramas de hotfix → `hotfix/nombre`.
- Se tendrán en cuenta la claridad de los mensajes de commit y la claridad del uso de ramas a la hora de evaluación

## Estructura del repositorio
```
proyecto-contable/
│── BE/ # Proyecto entero en BE
│
│── FE/ # Proyecto entero en FE
│
│── docker-compose.yml # Orquestación de FE y BE
│── .env # Variables de entorno necesarias para desplegar ambos contenedores
│── README.md # Documentación del proyecto
│── sample_data.xlsx # Datos de ejemplo (proveídos por el instructor)
│── catalogo_cuentas.json # Catálogo de cuentas de El Salvador (Investigados por el alumno)
```

## Entregables del proyecto
- Código fuente en GitHub con Gitflow y Gitmoji, entregado a travéz del repositorio de github, nombrar el repositorio `sc-grupo<#grupo>-proyecto-1`. Ej: `sc-grupo1-proyecto-1`  
- Catálogo de cuentas contables de El Salvador en JSON/CSV.
- Archivo Excel de ejemplo para pruebas.
- Instrucciones de instalación y ejecución (`README.md`).
- Capturas de pantalla del sistema en funcionamiento.

---

## Consideraciones
- Priorizar claridad de código y documentación.
- Utilizar correctamente [gitflow](https://www.atlassian.com/es/git/tutorials/comparing-workflows/gitflow-workflow)
- Dockerizar ambos proyectos y levantar con docker-compose