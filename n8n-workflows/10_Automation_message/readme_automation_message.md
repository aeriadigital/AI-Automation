# 🤖 Chatbot de Atención al Cliente — n8n + Gemini

Workflow de n8n que implementa un asistente virtual para un salón de belleza, integrado como widget de chat en una página web Next.js desplegada en Vercel.

## Arquitectura

```
Chat Trigger (Webhook)
        ↓
    AI Agent
    ↙       ↘
Gemini     Simple Memory
(LLM)      (contexto)
```

## Nodos

| Nodo | Tipo | Función |
|------|------|---------|
| Chat asistente para página web | `chatTrigger` | Recibe mensajes del widget embebido |
| AI Agent | `agent` | Orquesta el LLM con el system prompt del negocio |
| Google Gemini Chat Model | `lmChatGoogleGemini` | Modelo de lenguaje que genera las respuestas |
| Simple Memory | `memoryBufferWindow` | Mantiene historial de los últimos 15 intercambios |

## Requisitos

- n8n (local o Cloud)
- API Key de Google Gemini ([obtener en Google AI Studio](https://aistudio.google.com/))
- Sitio web con soporte para módulos ES (Next.js, Vite, etc.)

## Instalación

### 1. Importar el workflow

En n8n: **Workflows → Import from file** → seleccioná `automatizacion-message.json`

### 2. Configurar credenciales

En el nodo **Google Gemini Chat Model**:
1. Abrí el nodo
2. En **Credential** → Create new
3. Pegá tu Google Gemini API Key
4. Guardá

### 3. Configurar el Chat Trigger

En el nodo **Chat asistente para página web**:
- `Make Chat Publicly Available`: activado
- `Mode`: Webhook
- `Allowed Origins (CORS)`: agregá tu dominio (ej. `https://tu-sitio.vercel.app`)

### 4. Activar el workflow

Usá el toggle **Active** en la esquina superior derecha. Una vez activo, copiá la **Chat URL** que aparece en el nodo trigger.

## Integración en la página web

Agregá este código a tu componente principal (Next.js):

```tsx
"use client"

import { useEffect } from "react"

export default function Home() {
  useEffect(() => {
    const link = document.createElement("link")
    link.rel = "stylesheet"
    link.href = "https://cdn.jsdelivr.net/npm/@n8n/chat/dist/style.css"
    document.head.appendChild(link)

    const script = document.createElement("script")
    script.type = "module"
    script.innerHTML = `
      import { createChat } from 'https://cdn.jsdelivr.net/npm/@n8n/chat/dist/chat.bundle.es.js';
      createChat({
        webhookUrl: 'https://TU-INSTANCIA.app.n8n.cloud/webhook/TU-WEBHOOK-ID/chat',
        mode: 'window',
        defaultLanguage: 'en',
        initialMessages: ['¡Hola! 👋', '¿En qué puedo ayudarte?'],
        i18n: {
          en: {
            title: 'Nombre del negocio',
            subtitle: 'Tu asistente virtual, disponible 24/7.',
            inputPlaceholder: 'Escribí tu consulta...',
            getStarted: 'Nueva conversación',
            footer: ''
          }
        }
      });
    `
    document.body.appendChild(script)

    return () => {
      document.head.removeChild(link)
      document.body.removeChild(script)
    }
  }, [])

  return <main>{/* tu contenido */}</main>
}
```

> ⚠️ Reemplazá `webhookUrl` con la URL real del nodo Chat Trigger. Nunca uses `localhost` en producción.

## Personalización del System Prompt

El comportamiento del agente se define en el nodo **AI Agent → Options → System Message**. Podés adaptarlo a cualquier negocio modificando:

- Nombre y ubicación del negocio
- Lista de servicios y precios
- Horarios de atención
- Formas de pago
- Datos de contacto para reservas

## Errores frecuentes

| Error | Causa | Solución |
|-------|-------|----------|
| Chat no responde | `webhookUrl` apunta a `localhost` | Reemplazar por URL pública de n8n Cloud |
| Error CORS | Dominio no autorizado en el trigger | Agregar dominio en `Allowed Origins` del nodo |
| 401 / 403 en Gemini | Credencial inválida o expirada | Regenerar API Key en Google AI Studio |
| Chat no aparece en preview de v0 | Sandbox restringe módulos ES externos | Usar `useEffect` con `createElement` en lugar de `<Script>` |

## Stack

- [n8n](https://n8n.io/) — motor de automatización
- [Google Gemini](https://ai.google.dev/) — modelo de lenguaje
- [@n8n/chat](https://www.npmjs.com/package/@n8n/chat) — widget de chat embebible
- [Next.js](https://nextjs.org/) + [Vercel](https://vercel.com/) — frontend
