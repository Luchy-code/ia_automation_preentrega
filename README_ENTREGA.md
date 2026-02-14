
# IA Automation: Registro Automatizado de Divisas 📈

Este proyecto consiste en un flujo de automatización desarrollado en **n8n** diseñado para el monitoreo, registro y análisis inteligente del valor del dólar oficial en Argentina.

## 📋 Propósito

El sistema automatiza la extracción diaria de datos de compra y venta de divisas, gestiona el almacenamiento dinámico en Google Sheets y utiliza modelos de lenguaje (LLM) para generar reportes financieros enviados por correo electrónico.

---

## ⚙️ Arquitectura del Flujo

### 1. Extracción y Validación

* **Trigger (Schedule):** El flujo se activa de lunes a viernes a las 7:00 AM.
* **Llamada a API:** Consulta al endpoint oficial de **DolarApi** para obtener los valores vigentes.
* **Gestor de Errores:** Si la API devuelve un error (ej. `404` o `Axios Error`), se registra en la hoja `error_log` con prioridad alta y se notifica al usuario.

---

### 2. Lógica de Almacenamiento (Google Sheets)

El sistema organiza los datos de forma inteligente para evitar la saturación de archivos:

* **Gestión de Hojas:** Identifica el mes corriente (ej. "February-2026") para separar los registros mensualmente.
* **Lógica de Inicio de Mes:**
  * **Si es día 1:** Crea una nueva pestaña y define los encabezados mediante un nodo *Edit Fields*.
  * **Si no es día 1:** Añade una nueva fila con la fecha, hora, valores de compra/venta y estado "OK".
* **Control de Duplicados:** Valida si ya existe un registro para el día actual antes de escribir, evitando datos redundantes.

---

### 3. Inteligencia Artificial (LLM via GROQ)

Se integran dos nodos de procesamiento de lenguaje natural para el análisis de datos:

* **Analista de Clasificación:** Determina si el valor **SUBIÓ**, **BAJÓ** o **SE MANTUVO** comparándolo con la jornada anterior.
* **Generador de Reportes:** Redacta un análisis financiero breve (máximo 5 líneas) resaltando estabilidad, presión alcista o retrocesos según la variación detectada.

---

## 📧 Salida y Notificaciones

El flujo concluye con el envío de un correo electrónico que incluye:

1. **Cuadro Informativo:** Resumen de la operación con valores de compra y venta.
2. **Análisis IA:** La clasificación y el reporte técnico generado por el LLM.
3. **Alertas Técnicas:** Notificaciones inmediatas en caso de errores de API o registros duplicados.

> **Nota Técnica:** La clasificación y el reporte de la IA son dinámicos y exclusivos para el cuerpo del mail; no se almacenan en el Excel para mantener la planilla limpia.

---

**Desarrollado por:** Luciana Oviedo

---
