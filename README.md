# AI-Automation

Colección de workflows de n8n documentados, construidos durante mi proceso de aprendizaje.

## Estado
Todos los workflows fueron probados y ejecutados con éxito ✅

## Nivel
Principiante — ejercicios progresivos del curso de n8n.

---

## Workflows disponibles

| # | Carpeta | Nodos clave | Descripción breve |
|---|---------|-------------|-------------------|
| 01 | [01_fecha-y-hora](./n8n-workflows/01_fecha-y-hora/) | DateTime · Code · If · Gmail · Slack | Detecta si es fuera de horario y envía respuesta automática |
| 02 | [02_retry](./n8n-workflows/02_retry/) | Form Trigger · Google Sheets · If · Loop | Formulario con verificación en Google Sheets y reintento |
| 03 | [03_http request traductor](./n8n-workflows/03_http%20request%20traductor/) | HTTP Request · Gemini · If · Gmail | Obtiene chiste aleatorio de API pública y lo traduce al español |
| 04 | [04_envio-formulario](./n8n-workflows/04_envio-formulario/) | Form Trigger · Gemini · Gmail | Formulario web que genera email personalizado con IA |
| 05 | [05_retry_backoff](./n8n-workflows/05_retry_backoff/) | HTTP Request · If · Wait · Set | Reintentos automáticos con espera exponencial (2s → 4s → 8s) |
| 06 | [06_Get receta con Logs y documentacion](./n8n-workflows/06_Get%20receta%20con%20Logs%20y%20documentacion/) | HTTP Request · Code · If | Bifurcación por rating con logging estructurado en cada nodo |
| 07 | [07_ Historia de Personaje One Piece por Email](./n8n-workflows/07_%20Historia%20de%20Personaje%20One%20Piece%20por%20Email/) | Webhook · HTTP Request · Gemini · Gmail | Historia de personaje generada con IA y enviada por email HTML |
| 08 | [08_registro de logs](./n8n-workflows/08_registro%20de%20logs/) | Google Sheets · HTTP Request · DateTime | Guarda cada ejecución como fila de log en Google Sheets |

---

## Estructura del repositorio

```
AI-Automation/
├── README.md
└── n8n-workflows/
    ├── 01_fecha-y-hora/
    ├── 02_retry/
    ├── 03_http request traductor/
    ├── 04_envio-formulario/
    ├── 05_retry_backoff/
    ├── 06_Get receta con Logs y documentacion/
    ├── 07_ Historia de Personaje One Piece por Email/
    └── 08_registro de logs/
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
