# 📊 Análisis Automático de Sentimiento de Tweets
**Proyecto Final — Automatización con n8n y LLMs**
Coderhouse — Curso AI Automation

---

## 📌 Descripción del proyecto

Este workflow automatiza el análisis de sentimiento de tweets
recibidos en una hoja de cálculo de Google Sheets. Cada vez que
se agrega una fila nueva, el sistema clasifica el tweet como
POSITIVO, NEGATIVO o NEUTRAL usando Gemini, y actúa según
el resultado: envía una alerta por email si es negativo, y
registra todos los casos en una hoja de historial.

---

## ⚙️ Requisitos previos

- n8n instalado localmente (v1.0+)
- Cuenta de Google con acceso a:
  - Google Sheets API habilitada
  - Google Drive API habilitada
  - Gmail
- API Key de Google Gemini (Google AI Studio)
- Cuenta de Gmail para notificaciones

---

## 🔑 Credenciales necesarias

| Servicio | Tipo | Dónde obtenerla |
|---|---|---|
| Google Sheets Trigger | OAuth2 | Google Cloud Console |
| Google Sheets (acción) | OAuth2 | Google Cloud Console |
| Google Gemini | API Key | aistudio.google.com |
| Gmail | OAuth2 | Google Cloud Console |

> ⚠️ No se incluyen credenciales reales en este archivo.
> Configurar cada una desde n8n → Settings → Credentials.

---

## 📋 Estructura de la hoja de cálculo

### Hoja 1 — `Sentiment tweets` (tweets de entrada)

| Columna | Descripción |
|---|---|
| tweet_text | Texto del tweet |
| usuario | Nombre de usuario |
| fecha | Fecha del tweet |

### Hoja 2 — `Registro` (historial de procesamiento)

| Columna | Descripción |
|---|---|
| fecha | Fecha del tweet |
| usuario | Nombre de usuario |
| tweet_text | Texto del tweet |
| sentiment | POSITIVO / NEGATIVO / NEUTRAL |
| score | Nivel de confianza (0.0 a 1.0) |
| summary | Resumen del problema (solo negativos) |
| reasons | Razones identificadas (solo negativos) |
| suggested_reply | Respuesta sugerida (solo negativos) |
| accion | Acción tomada por el sistema |

> 🎨 Configurar formato condicional en columna `sentiment`:
> POSITIVO → verde | NEUTRAL → amarillo | NEGATIVO → rojo

---

## 🚀 Cómo importar el workflow

1. Abrís n8n en `http://localhost:5678`
2. Menú izquierdo → **Workflows → Import from file**
3. Seleccionás `workflow.json`
4. El workflow se importa con todos los nodos conectados

---

## 🔧 Configuración paso a paso

### 1. Credencial Google Sheets Trigger
- n8n → Settings → Credentials → New
- Tipo: **Google Sheets Trigger OAuth2**
- Ingresás Client ID y Client Secret de Google Cloud
- URI de callback: `http://localhost:5678/rest/oauth2-credential/callback`
- Click en Sign in with Google → aceptás permisos de Sheets y Drive

### 2. Credencial Google Sheets (acción)
- Mismo proceso, tipo: **Google Sheets OAuth2**
- Podés reutilizar la misma app de Google Cloud

### 3. Credencial Google Gemini
- Entrás a aistudio.google.com
- Get API Key → Create API Key
- En n8n → tipo: **Google Gemini(PaLM) API**
- Pegás la API Key

### 4. Credencial Gmail
- n8n → tipo: **Gmail OAuth2**
- Misma app de Google Cloud, aceptás permisos de Gmail

### 5. Configurar el nodo Trigger
- Abrís el nodo `Google Sheets Trigger`
- Seleccionás tu spreadsheet y la pestaña `Sentiment tweets`
- Evento: `Row Added`
- Poll interval: Every Minute

### 6. Configurar el nodo Gmail
- Abrís `📧 Gmail: Enviar Alerta Negativo`
- Reemplazás el campo `To` con tu email real

---

## ▶️ Cómo ejecutar y probar

### Activar el workflow
- Toggle arriba a la derecha → **Active**
- El trigger empieza a hacer polling cada 1 minuto

### Tweets de prueba

Agregás estas filas una por una en `Sentiment tweets` y esperás ~1 minuto entre cada una:

| tweet_text | usuario | fecha |
|---|---|---|
| Pésimo servicio, llevo 3 días esperando y nadie contesta | TestUser1 | 28/03/2026 |
| Hoy recibí mi pedido, todo en orden | TestUser3 | 28/03/2026 |
| Usé el producto por primera vez hoy | TestNeutral | 29/03/2026 |

### Qué verificar
- Tweet negativo → llega email a tu Gmail + fila roja en Registro
- Tweet positivo → fila verde en Registro, sin email
- Tweet neutral → fila amarilla en Registro, sin email
- Pestaña Executions → cada ejecución automática aparece listada

---

## 🛡️ Manejo de errores

El workflow contempla dos escenarios de fallo:

**LLM devuelve formato inesperado:** ambos nodos Code tienen fallback por keywords. Si el JSON falla, el sistema detecta las palabras NEGATIVO/POSITIVO/NEUTRAL en el texto crudo y clasifica igualmente.

**Rate limit de Gemini:** el plan gratuito tiene límite de 20 requests/minuto y 50 requests/día. Si se supera, la ejecución falla con error "Too many requests". Solución: esperar 1 minuto o usar una API Key de otra cuenta.

---

## 📁 Archivos incluidos en el .zip

| Archivo | Descripción |
|---|---|
| `workflow_ENTREGA_FINAL.json` | Workflow exportado desde n8n |
| `prompts.txt` | Prompts usados en LLM 1 y LLM 2 |
| `Documentacionyevidencias.pdf` | Diagrama y descripción técnica y Capturas de pantalla |
| `README.md` | Este archivo |

---

## ⚠️ Nota sobre credenciales

El archivo `workflow.json` **no contiene credenciales reales**. Las referencias a credenciales son IDs internos de n8n que no funcionan en otra instancia. Cada evaluador debe crear sus propias credenciales siguiendo esta guía.
