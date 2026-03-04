# 09 · — Historia de Personaje One Piece por Email

**Descripción:** Recibe una solicitud vía Webhook con nombre y email del destinatario, consulta la API de One Piece para obtener datos de un personaje, genera una historia personalizada en estilo storytelling con Gemini y la envía por email con diseño HTML.

**Estado:** ✅ Exitoso  
**Nivel:** Principiante  
**Nodos usados:** `Webhook`, `HTTP Request`, `If`, `Google Gemini`, `HTML`, `Gmail`  
**Aprendizaje clave:** Cómo combinar un webhook como disparador, consumir una API con autenticación Bearer, generar contenido narrativo con IA y enviarlo en un email con diseño HTML personalizado.

## Credenciales requeridas
- Bearer Token para la API de One Piece (configurar en n8n como `httpBearerAuth`)
- Google Gemini API (configurar con tu propia API key)
- Gmail (configurar con tu propia cuenta en n8n)

## API utilizada
- [One Piece API](https://api.api-onepiece.com/v2/characters/en/2) — requiere Bearer Token

## Body esperado en el Webhook
```json
{
  "email": "destinatario@ejemplo.com",
  "name": "Nombre del usuario"
}
```

## Cómo funciona
1. Webhook recibe POST en `/xxxxx` con email y nombre
2. Consulta el personaje ID `2` de la API de One Piece
3. Verifica que el email no esté vacío
4. **Si tiene email** → Gemini genera historia en estilo storytelling → se arma el HTML → se envía por Gmail
5. **Si no tiene email** → el flujo se detiene (rama false sin acción)

## Diseño del email
Header oscuro con nombre del personaje, cuerpo con la historia generada, firma **** y footer que menciona n8n + Gemini como generadores.
