# · Registro de Logs en Google Sheets

**Descripción:** Obtiene una receta desde una API pública, captura la fecha y hora de ejecución, arma un objeto de log estructurado y lo guarda como fila nueva en un Google Sheet. También lee los logs existentes antes de escribir para mantener trazabilidad del historial.

**Estado:** ✅ Exitoso  
**Nivel:** Principiante  
**Nodos usados:** `Manual Trigger`, `Google Sheets (read)`, `HTTP Request`, `DateTime`, `Set`, `Google Sheets (append)`  
**Aprendizaje clave:** Cómo registrar ejecuciones en Google Sheets como sistema de logging, capturar timestamps y serializar payloads completos como JSON string.

## Credenciales requeridas
- Google Sheets (configurar con tu propia cuenta de Google en n8n)

## API utilizada
- [DummyJSON Recipes](https://dummyjson.com/recipes/1) — pública, sin autenticación

## Google Sheet requerido
Crear una hoja con exactamente estos headers (respetando mayúsculas):

| fecha | Estado | Payload |
|-------|--------|---------|

> ⚠️ Los nombres de columna deben coincidir exactamente con los del nodo `Armar Objeto de Log`.

## Cómo funciona
1. Se ejecuta manualmente
2. Lee los logs existentes en el Sheet (si está vacío, continúa igual)
3. Obtiene la receta ID `1` desde DummyJSON
4. Captura la fecha y hora actual
5. Arma el objeto de log con `fecha`, `Estado` (fijo: "exitoso") y `Payload` (JSON completo de la receta)
6. Escribe una fila nueva al final del Sheet
