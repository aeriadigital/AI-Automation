# Horario de Oficina — Respuesta Automática por Email

**Descripción:** Detecta si el horario actual está fuera del horario de oficina (después de las 18hs, Argentina) y envía automáticamente un email de respuesta avisando que los operadores no están disponibles. Luego notifica en un canal de Slack.

**Estado:** ✅ Exitoso  
**Nivel:** Principiante  
**Nodos usados:** `Manual Trigger`, `DateTime`, `Code (JavaScript)`, `If`, `Gmail`, `Slack`  
**Aprendizaje clave:** Cómo combinar lógica condicional con zona horaria local y disparar acciones distintas según el resultado.

## Credenciales requeridas
- Gmail (configurar con tu propia cuenta en n8n)
- Slack (configurar con tu propio workspace)

## Cómo funciona
1. Obtiene la fecha y hora actual
2. Formatea la hora según zona horaria Argentina (`America/Argentina/Buenos_Aires`)
3. Evalúa si la hora es mayor o igual a las 18hs
4. Si es verdadero → envía email de respuesta automática + notifica en Slack
5. Si es falso → no ejecuta ninguna acción
