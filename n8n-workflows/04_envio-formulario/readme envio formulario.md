# 04 · Envío de Formulario — Email Personalizado con IA

**Descripción:** Captura datos de un cliente mediante un formulario web, genera un email personalizado usando Gemini (IA) para agendar una reunión, y lo envía automáticamente al correo ingresado.

**Estado:** ✅ Exitoso  
**Nivel:** Principiante  
**Nodos usados:** `Form Trigger`, `Wait`, `Google Gemini`, `Code (JavaScript)`, `Gmail`  
**Aprendizaje clave:** Cómo combinar un formulario con IA generativa para personalizar comunicaciones y automatizar el seguimiento de clientes.

## Credenciales requeridas
- Gmail (configurar con tu propia cuenta en n8n)
- Google Gemini API (configurar con tu propia API key)

## Cómo funciona
1. El cliente completa un formulario con nombre, apellido, email, presupuesto y tipo de automatización requerida
2. Espera 30 segundos (para evitar envíos inmediatos)
3. Gemini genera un email persuasivo y personalizado según la necesidad del cliente
4. Un nodo de código construye el asunto y el cuerpo del email con formato limpio
5. Se envía el email al correo ingresado en el formulario con un link para agendar reunión
