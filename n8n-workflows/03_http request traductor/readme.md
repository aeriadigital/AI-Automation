# HTTP Request Traductor — Chiste Aleatorio Traducido al Español

**Descripción:** Consume una API pública para obtener un chiste aleatorio en inglés, lo traduce al español usando Gemini, evalúa el contenido con una condición y envía el resultado por email.

**Estado:** ✅ Exitoso  
**Nivel:** Principiante  
**Nodos usados:** `Manual Trigger`, `HTTP Request`, `Google Gemini`, `If`, `Set (Edit Fields)`, `Gmail`  
**Aprendizaje clave:** Cómo consumir una API externa con HTTP Request, procesar la respuesta con IA y validar que el resultado no esté vacío antes de continuar el flujo.

## Credenciales requeridas
- Google Gemini API (configurar con tu propia API key)
- Gmail (configurar con tu propia cuenta en n8n)

## API utilizada
- [Official Joke API](https://official-joke-api.appspot.com/random_joke) — pública, sin autenticación

## Cómo funciona
1. Se ejecuta manualmente
2. Hace una petición HTTP a la API de chistes aleatorios
3. Gemini traduce el `setup` del chiste al español
4. Una condición evalúa si contiene texto 
5. **Si la traducción no está vacía** → extrae `setup` y `punchline` y envía por email
6. **Si está vacía** → pasa por el flujo alternativo (Edit Fields vacío)
