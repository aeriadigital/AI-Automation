# Pre-entrega n8n — Cat Fact Diario con Alerta por Email

**Autor:** Michel, Andrea  
**Versión:** 1.0 — Pre-entrega  
**Fecha:** Marzo 2026  
**API utilizada:** https://catfact.ninja/fact — pública, sin autenticación requerida

---

## Nombre del caso

**Cat Fact Diario con Alerta por Email**

Cada 6 horas el workflow consulta una API pública de datos curiosos sobre gatos, normaliza el dato recibido, evalúa su longitud y — si supera los 100 caracteres — envía una notificación automática por Gmail con formato HTML.

---

## Qué dispara el workflow (Trigger)

**Nodo:** `Trigger - Cada 6 Horas`  
**Tipo:** Schedule Trigger  
**Configuración:** Interval → Hours → cada 6 horas

El workflow se ejecuta automáticamente sin intervención manual. No depende de un evento externo sino de un ciclo de tiempo definido, lo que lo convierte en un monitor periódico pasivo.

---

## Descripción de cada nodo

### 1. `Trigger - Cada 6 Horas`
Dispara el workflow automáticamente cada 6 horas mediante el nodo Schedule Trigger. No requiere acción del usuario. Se activa solo cuando el workflow está en estado **Active** en producción.

### 2. `API - Cat Fact`
Realiza una petición HTTP GET a `https://catfact.ninja/fact`. La API responde con un objeto JSON que contiene dos campos: `fact` (el texto del dato curioso) y `length` (la longitud del texto en caracteres). No requiere autenticación ni headers adicionales.

### 3. `Set - Normalizar Fact`
Extrae y renombra los campos relevantes del response de la API para aplanar la estructura de datos. Genera cuatro campos limpios: `fact`, `longitud`, `timestamp` y `fuente`. Esto simplifica el acceso a los datos en los nodos siguientes y evita navegar estructuras anidadas en el IF y en el Gmail.

### 4. `IF - ¿Fact > 100 caracteres?`
Evalúa si el campo `longitud` es mayor a 100 usando la operación `Greater Than` sobre tipo `Number`. La rama **true** deriva al nodo Gmail; la rama **false** deriva al nodo NoOp. Tiene activado el flag **"Convert types where required"** para convertir automáticamente strings a números antes de comparar, evitando fallos silenciosos ante cambios de tipo en la API.

### 5. `Gmail — Notificación por Email`
Envía un email con formato HTML al destinatario configurado cuando la condición del IF es verdadera. El cuerpo incluye el fact destacado, la longitud en caracteres, la fuente y el timestamp de ejecución. La credencial Gmail OAuth2 está almacenada exclusivamente en el gestor de credenciales de n8n.

### 6. `NoOp - Fact Ignorado`
Cierra la rama false del condicional de forma explícita. No ejecuta ninguna acción pero permite que la ejecución quede registrada correctamente en los logs de n8n, indicando que el workflow corrió aunque no se haya enviado ningún email.

---

## Qué evalúa el condicional y por qué

**Condición:** `$json.longitud > 100`

Se evalúa la longitud del fact recibido porque los facts cortos tienden a ser triviales, mientras que los más extensos suelen contener información más detallada e interesante. El umbral de 100 caracteres es una convención arbitraria pero justificable que filtra el ruido y garantiza que solo se envíen notificaciones con contenido de valor.

La elección de este condicional también demuestra manejo de datos de tipo numérico provenientes de una API de texto, que es uno de los casos más comunes en workflows reales.

---

## Cómo se configuró la notificación

**Servicio:** Gmail  
**Autenticación:** OAuth2 (configurada en el gestor de credenciales de n8n)  
**Tipo de mensaje:** HTML  
**Asunto:** `Dato curioso del día {{ $('Trigger - Cada 6 Horas').item.json['Readable date'] }}`

El cuerpo del email usa una plantilla HTML con estilos inline que incluye:
- Header con título y badge destacado
- Caja principal con el texto del fact en itálica
- Sección de metadata con longitud, fuente y timestamp
- Footer con aclaración de mensaje automático

Las expresiones de n8n (`{{ $json.fact }}`, `{{ $json.longitud }}`, etc.) se resuelven en tiempo de ejecución e insertan los datos normalizados por el nodo Set.

---

## Buenas prácticas aplicadas

**Credenciales:**
- La credencial Gmail OAuth2 está almacenada únicamente en el gestor de credenciales de n8n.
- No se expone ninguna credencial en el archivo JSON exportado ni en este README.
- Para replicar el workflow: configurar credencial en n8n → Gmail OAuth2 → seleccionar la cuenta correspondiente.

**Logs e idempotencia:**
- Ambas ramas del IF están cerradas explícitamente (Gmail y NoOp), garantizando que toda ejecución quede registrada en el Execution Log de n8n independientemente del resultado del condicional.
- El nodo Set normaliza los datos antes del IF, lo que hace el workflow más predecible y fácil de depurar ante errores.

**Estructura:**
- Cada nodo tiene nombre descriptivo para identificación visual rápida.
- Cada nodo tiene un Sticky Note con descripción, configuración y advertencias relevantes directamente en el canvas.
- El workflow está en estado `inactive` en el JSON exportado para evitar ejecuciones accidentales al importarlo.

---

## Estructura del ZIP

```
/pre-entrega
│── workflow_preentrega.json
│── README_preentrega.md
└── /evidencias
    └── captura_1.png
```

---

> **Nota:** Las credenciales de Gmail no están incluidas en el JSON exportado. Al importar el workflow en n8n, configurar manualmente la credencial Gmail OAuth2 en el nodo `📧 Gmail — Notificación por Email`.
