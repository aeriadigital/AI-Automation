# AI-Automation

Colección de workflows de n8n documentados, construidos durante mi proceso de aprendizaje en automatización con IA.

## Estado
Todos los workflows fueron probados y ejecutados con éxito ✅

## Nivel
Principiante — ejercicios progresivos del curso de n8n.

---

## Workflows disponibles

| # | Nombre | Nodos clave | Descripción breve |
|---|--------|-------------|-------------------|
| 01 | [Horario de Oficina](./n8n-workflows/03_horario-oficina/) | DateTime · Code · If · Gmail · Slack | Detecta si es fuera de horario y envía respuesta automática |
| 02 | [Retry con Sheets](./n8n-workflows/05_retry/) | Form · Google Sheets · If · Loop | Formulario con verificación en Google Sheets y reintento |
| 03 | [HTTP Request Traductor](./n8n-workflows/06_http-request-traductor/) | HTTP Request · Gemini · If · Gmail | Obtiene chiste aleatorio de API pública y lo traduce al español |
| 04 | [Envío de Formulario](./n8n-workflows/04_envio-formulario/) | Form Trigger · Gemini · Gmail | Formulario web que genera email personalizado con IA |
| 05 | [Retry Backoff](./n8n-workflows/07_retry-backoff/) | HTTP Request · If · Wait · Set | Reintentos automáticos con espera exponencial (2s → 4s → 8s) |
| 06 | [U3 — Recetas con Logs](./n8n-workflows/08_u3-logs-documentacion/) | HTTP Request · Code · If | Bifurcación por rating con logging estructurado en cada nodo |
| 07 | [Clase 7 — One Piece Email](./n8n-workflows/09_clase7-onepiece-historia/) | Webhook · HTTP Request · Gemini · Gmail | Historia de personaje generada con IA y enviada por email HTML |
| 08 | [Clase 8 — Registro de Logs](./n8n-workflows/10_clase8-registro-logs/) | Google Sheets · HTTP Request · DateTime | Guarda cada ejecución como fila de log en Google Sheets |

---

## Estructura del repositorio

```
AI-Automation/
├── README.md
└── n8n-workflows/
    ├── 01_horario-oficina/
    │   ├── workflow.json
    │   └── README.md
    ├── 02_envio-formulario/
    │   ├── workflow.json
    │   └── README.md
    └── ...
```

Cada carpeta contiene:
- El archivo `.json` listo para importar en n8n
- Un `README.md` con descripción, nodos usados, credenciales requeridas y flujo detallado

---

## Cómo importar un workflow

1. Abrí n8n
2. Menú → **Import from File**
3. Seleccioná el archivo `.json` de la carpeta deseada
4. Configurá las credenciales indicadas en el README de cada workflow

---

## Credenciales frecuentes

La mayoría de los workflows usan alguna combinación de estas credenciales. Configurarlas una vez en n8n y reutilizarlas:

- **Gmail OAuth2** — para envío de emails
- **Google Sheets OAuth2** — para lectura y escritura en planillas
- **Google Gemini API** — para generación de contenido con IA
- **HTTP Bearer Auth** — para APIs con autenticación por token

---

*Construido con n8n · Gemini · APIs públicas*
