# 📊 Clase 8 — Registro de Logs con Manejo de Errores

Workflow de n8n para automatización resiliente: consume una API externa, valida idempotencia y registra el resultado (exitoso o error) en Google Sheets, con alerta por Gmail en caso de fallo.

---

## 🗺️ Arquitectura del flujo

```
Trigger Manual
    └── 🌐 HTTP Request (DummyJSON API)
            ├── [rama OK]  → 📋 Leer Logs → Idempotencia
            │                       ├── [duplicado] → 🛑 Stop & Error
            │                       └── [nuevo]     → 🕐 Fecha/Hora → ✅ Armar Log → ✅ Guardar en Sheet
            │
            └── [rama ERROR] → ⏳ Delay → 🔴 Armar Log Error → 🔴 Guardar en Sheet → 📧 Gmail Alert
```

---

## 🧩 Nodos y su función

| Nodo | Tipo | Descripción |
|---|---|---|
| 🚀 Trigger Manual | Manual Trigger | Dispara el workflow manualmente |
| 🌐 Obtener Receta (DummyJSON) | HTTP Request | Consume `https://dummyjson.com/recipes/roto`. Retry 3x cada 2s. On error: continúa por rama error |
| 📋 Leer Logs Existentes | Google Sheets | Lee todos los registros previos para chequeo de idempotencia. `Always Output Data: true` |
| Idempotencia | IF | Evalúa si el `order_id` ya existe en el sheet. True → duplicado, False → continúa |
| 🛑 Evitar duplicados | Stop & Error | Detiene el flujo si el registro ya existe |
| 🕐 Capturar Fecha y Hora | DateTime | Captura timestamp actual |
| ✅ Armar Log Exitoso | Set | Construye el objeto: `fecha`, `Estado=exitoso`, `Payload` (JSON stringify), `order_id` |
| ✅ Guardar Log en Sheet | Google Sheets (append) | Escribe la fila de éxito |
| ⏳ Delay antes de loggear error | Wait | Pausa de 5s antes de procesar el error |
| 🔴 Armar Log Error | Set | Construye el objeto: `fecha`, `Estado=error`, `Payload` (mensaje), `error_detalle` (código + status) |
| 🔴 Guardar Error en Sheet | Google Sheets (append) | Escribe la fila de error |
| 📧 Alerta Error por Gmail | Gmail | Envía email HTML con detalle del fallo |

---

## 🏗️ Estructura del Google Sheet

**Nombre del archivo:** `Registro de Logs`  
**Hoja:** `Hoja 1`

| Columna | Tipo | Descripción |
|---|---|---|
| `fecha` | string | ISO timestamp del evento |
| `Estado` | string | `exitoso` o `error` |
| `Payload` | string | JSON stringify de la respuesta (éxito) o mensaje de error |
| `order_id` | string | ID único del registro (solo en rama exitosa) |
| `error_detalle` | string | Código de error + HTTP status (solo en rama error) |

---

## ⚙️ Configuración requerida

### 1. Credenciales

Antes de importar el workflow, necesitás configurar estas credenciales en n8n:

- **Google Sheets OAuth2** → conectar tu cuenta de Google con acceso a Sheets
- **Gmail OAuth2** → conectar tu cuenta de Gmail para el envío de alertas

### 2. Variables a reemplazar

Buscá y reemplazá estos placeholders en el JSON antes de importar:

| Placeholder | Qué poner |
|---|---|
| `YOUR_SPREADSHEET_ID` | El ID de tu Google Sheet (está en la URL entre `/d/` y `/edit`) |
| `YOUR_EMAIL@gmail.com` | Tu dirección de email para recibir alertas |
| `YOUR_CREDENTIAL_ID` | Se asigna automáticamente al vincular credenciales en n8n |
| `YOUR_INSTANCE_ID` | Se genera automáticamente al importar |

### 3. Pasos de configuración

1. Importar el workflow desde `clase_8_registro_logs.json` en n8n
2. Abrir los nodos de Google Sheets y vincular tu credencial OAuth2
3. Actualizar el `documentId` con el ID de tu spreadsheet
4. Abrir el nodo Gmail y vincular tu credencial + actualizar el email destinatario
5. Crear el Google Sheet con las columnas: `fecha`, `Estado`, `Payload`, `order_id`, `error_detalle`
6. Ejecutar con el Trigger Manual

---

## 🔑 Conceptos clave implementados

**Idempotencia**: el flujo chequea si el `order_id` ya existe antes de escribir, evitando registros duplicados ante re-ejecuciones.

**Manejo de errores con continuación**: el nodo HTTP tiene `onError: continueErrorOutput`, lo que permite que el flujo siga por la rama de error en lugar de detenerse abruptamente.

**Retry automático**: 3 reintentos con 2 segundos de espera entre cada uno antes de derivar al error output.

**Structured logging**: tanto el éxito como el error generan un objeto con campos estandarizados antes de persistirlos.


---

## 🏷️ Tags

`n8n` · `automatización` · `google-sheets` · `gmail` · `error-handling` · `idempotencia` · `logging` · `curso-ai-automation`
