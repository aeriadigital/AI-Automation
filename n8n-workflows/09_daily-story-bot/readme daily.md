# Historia Diaria con Gemini — n8n Workflow

Workflow de automatización construido en n8n que genera y envía por email una micro-historia con impacto emocional, usando inteligencia artificial. Se ejecuta automáticamente todos los días a las 15:00 hs (hora de Argentina).

---

## Flujo

```
Schedule Trigger → HTTP Request → Gemini 2.5 Flash → Gmail
```

Cada nodo tiene una responsabilidad clara:

- **Schedule Trigger** — dispara el workflow a las 15:00 hs (UTC-03:00)
- **HTTP Request** — consume una API pública y obtiene el contenido base
- **Gemini 2.5 Flash** — transforma ese contenido en una historia en español
- **Gmail** — envía la historia al destinatario configurado

---

## Tecnologías

- [n8n](https://n8n.io/) — plataforma de automatización
- [Google Gemini API](https://aistudio.google.com/) — modelo de lenguaje (gemini-2.5-flash)
- [DummyJSON](https://dummyjson.com/) — API pública simulada
- Gmail — canal de entrega vía OAuth 2.0

---

## Requisitos

- Cuenta en n8n (cloud o self-hosted)
- API Key de Google Gemini → [obtener en Google AI Studio](https://aistudio.google.com/)
- Cuenta de Gmail conectada via OAuth 2.0

---

## Configuración

### 1. Importar el workflow

En n8n: `Workflows → Import from file` → seleccionar `workflow.json`

### 2. Configurar credenciales

Antes de ejecutar, necesitás crear dos credenciales en `Settings → Credentials`:

**Gemini API Key**
```
Tipo: Google Gemini(PaLM) Api
Campo: API Key
Valor: tu clave de Google AI Studio (empieza con AIzaSy...)
```

**Gmail OAuth 2.0**
```
Tipo: Gmail OAuth2
Seguir el flujo de autorización de Google
```

### 3. Actualizar el nodo Gmail

Abrir el nodo **Send a message** y reemplazar el campo `Send To` con tu dirección de email.

### 4. Activar el workflow

Desde el panel principal, activar el toggle del workflow. Se ejecutará automáticamente a las 15:00 hs todos los días.

---

## Seguridad

### Métodos de autenticación por servicio

| Servicio | Método | Notas |
|---|---|---|
| DummyJSON | Ninguno | API pública, sin datos sensibles |
| Gemini API | API Key | Encriptada en n8n con AES-256 |
| Gmail | OAuth 2.0 | Token con renovación automática |

### Almacenamiento de credenciales

n8n encripta todas las credenciales con AES-256 en su base de datos. El archivo `workflow.json` solo contiene IDs de referencia, nunca las claves en texto plano. Es seguro compartirlo y subir a repositorios públicos.

### Política de rotación

- **Gemini API Key** → rotación manual cada 90 días desde [Google AI Studio](https://aistudio.google.com/)
- **Gmail OAuth token** → renovación automática via refresh token, sin intervención manual
- **DummyJSON** → no aplica

Procedimiento de rotación para Gemini:
1. Generar nueva API Key en Google AI Studio
2. Actualizar en n8n → Settings → Credentials
3. Ejecutar el workflow para verificar
4. Revocar la clave anterior

### Control de acceso

Se aplica el principio de mínimo privilegio:

- **Owner** del workspace → puede ver y editar credenciales
- **Member** del workspace → puede ejecutar el workflow sin acceder a las claves
- Recomendación para equipos: mantener credenciales separadas para entornos DEV y PROD

---

## Estructura del prompt

El nodo de Gemini usa un prompt en dos capas:

**System message** — define el rol del modelo: narrador sensible, tono cálido, sin clichés, sin emojis, sin markdown.

**User message** — entrega el contenido de la API y las instrucciones de estructura:
1. Introducción con escena concreta
2. Desarrollo con conflicto o insight
3. Cierre emocional
4. Frase inspiradora final en español (máximo 12 palabras)

Extensión esperada: entre 150 y 300 palabras.

---

## Notas de implementación

- El pinData del Schedule Trigger fue eliminado del archivo público. Al importar, n8n generará timestamps reales en cada ejecución.
- El timezone está configurado en `America/Argentina/Buenos_Aires (UTC-03:00)`.
- El workflow está marcado como `inactive` por defecto. Activarlo manualmente después de configurar las credenciales.

---

## Contexto académico

Este workflow fue desarrollado como parte de la **Actividad Práctica 2 — Unidad 3** del curso de AI Automation, con foco en autenticación segura de APIs, gestión de credenciales y políticas de control de acceso en workflows de automatización.
