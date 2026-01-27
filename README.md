# ia_automation_preentrega
``` markdown

# Documentación Técnica: Automatización de Registro de Divisas

Desarrollado por: Luciana Oviedo
Propósito: Registrar diariamente el valor de compra-venta del dólar oficial y gestionar errores de API en Google Sheets.

---

## 1. Arquitectura del Flujo

El sistema opera mediante un disparador cronológico que activa una consulta a la API de DolarApi. El flujo se bifurca mediante lógica condicional para manejar excepciones de conexión o procesar los datos según el día del mes.

## 2. Especificación de Nodos

* **Trigger (Schedule)**: Ejecución automática de lunes a viernes a las 7:00 AM.
* **Llamada a API**: Consulta tipo HTTP al endpoint oficial para obtener valores de compra y venta.
* **Gestor de Errores**: Valida la existencia de la propiedad error. Si es True, registra el incidente (Fecha, Hora, Tipo de Error, Prioridad Alta) en la hoja error_log del documento Pre_entrega-sheet y envía una notificación.

### Procesamiento de Datos (Caso Exitoso)

* **Definición de Hoja**: Se genera un nombre dinámico basado en el mes y año corriente (ej. "January-2026").

* **Lógica de Inicio de Mes**: Un condicional verifica si el día del mes es igual a 1.

  * **Si es día 1**: Se crea una nueva pestaña con el nombre del mes. Se utiliza un nodo Edit Fields para definir los encabezados de la tabla y se inserta el primer registro.
  * **Si no es día 1**: Se identifica la hoja correspondiente al mes actual y se añade una nueva fila con los datos recolectados.

* **Variables Registradas**: Se capturan y formatean la Fecha, Hora (formato HH:mm:ss), Valor Dólar Compra, Valor Dólar Venta y un estado de confirmación "ok" -> encabezado del cuadro de excel.

## 3. Salida y Notificaciones

Independientemente del resultado, el sistema finaliza con una notificación vía correo electrónico. En caso de éxito, el cuerpo del mensaje incluye un cuadro informativo en formato HTML con el resumen de la operación diaria; en caso de fallo, se reporta el detalle del error técnico.

```
