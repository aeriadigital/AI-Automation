Ejercicio en desarrollo. Base para prueba de loops.

# 05 · Retry — Formulario con Verificación en Google Sheets y Reintento

**Descripción:** Extiende el flujo de formulario con IA incorporando una verificación contra Google Sheets antes de generar el email. Si el registro existe, procede con la personalización vía Gemini. Si no, ejecuta un flujo alternativo con lógica en Python, condición secundaria y un loop de reintento.

**Estado:** ✅ Exitoso  
**Nivel:** Principiante  
**Nodos usados:** `Form Trigger`, `Wait`, `Google Sheets`, `If`, `Google Gemini`, `Code (JavaScript)`, `Code (Python)`, `If`, `Set (Edit Fields)`, `Loop Over Items`, `Gmail`  
**Aprendizaje clave:** Cómo agregar bifurcaciones con lógica condicional, consultar una hoja de cálculo como fuente de datos y construir un loop de reintento dentro de un workflow.

## Credenciales requeridas
- Gmail (configurar con tu propia cuenta en n8n)
- Google Gemini API (configurar con tu propia API key)
- Google Sheets (configurar con tu propia cuenta de Google)

## Cómo funciona
1. El cliente completa el formulario de contacto
2. Espera 30 segundos
3. Consulta Google Sheets para verificar si el registro existe
4. **Si existe** → Gemini genera el email personalizado → se arma y envía
5. **Si no existe** → ejecuta código Python → evalúa condición secundaria → edita campos → entra en loop de reintento
