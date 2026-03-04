# 07 · Retry Backoff — Reintentos con Espera Exponencial

**Descripción:** Llama a una API de zona horaria y maneja fallos automáticamente con hasta 3 reintentos y espera exponencial (2s → 4s → 8s). Distingue errores de red de respuestas HTTP 200 con body inválido.

**Estado:** ✅ Exitoso  
**Nivel:** Principiante  
**Nodos usados:** `Manual Trigger`, `Set`, `HTTP Request`, `If`, `Wait`  
**Aprendizaje clave:** Cómo implementar retry con backoff exponencial en n8n, manejar errores de red y validar el contenido de la respuesta por separado.

## API utilizada
- [TimeAPI](https://timeapi.io/api/Time/current/zone?timeZone=America/Argentina/Buenos_Aires) — pública, sin autenticación

> ⚠️ El workflow tiene la URL seteada con `ZonaInvalida` para forzar errores durante el ejercicio. Cambiar a una zona válida (ej: `America/Argentina/Buenos_Aires`) para uso real.

## Cómo funciona

```
Llamada API
  ├── Success → ¿Tiene dateTime?
  │               ├── true  → Log Éxito ✅
  │               └── false → Preparar Reintento
  └── Error   → Preparar Reintento
                    └── ¿Reintentar? (máx. 3)
                          ├── true  → Cálculo Backoff → Wait → reinicia
                          └── false → Log Fallo Final ✅
```

## Detalles técnicos
- Espera inicial: 2000ms, se duplica en cada reintento (`espera * 2`)
- El nodo `Wait` convierte ms a segundos (`espera / 1000`)
- `error_msg` usa operador `??` para capturar cualquier forma de error
- Ambos caminos de fallo (red y body inválido) convergen en el mismo nodo de reintento
