# 08 · U3 — Recetas con Logs y Documentación

**Descripción:** Obtiene una receta desde una API pública, evalúa su rating y bifurca el flujo: si es destacada (≥ 4.5) busca sus comentarios, si no busca recetas alternativas de tipo dessert. Cada paso registra logs estructurados para trazabilidad completa.

**Estado:** ✅ Exitoso  
**Nivel:** Principiante  
**Nodos usados:** `Manual Trigger`, `HTTP Request`, `Code (JavaScript)`, `If`  
**Aprendizaje clave:** Cómo estructurar logs en cada etapa del flujo, manejar errores con `continueErrorOutput` y construir bifurcaciones basadas en datos dinámicos de la respuesta.

## APIs utilizadas
- [DummyJSON Recipes](https://dummyjson.com/recipes/1) — pública, sin autenticación
- [DummyJSON Comments](https://dummyjson.com/comments/post/{id}) — pública, sin autenticación
- [DummyJSON Recipes by tag](https://dummyjson.com/recipes?tag=dessert) — pública, sin autenticación

## Cómo funciona

```
GET Receta
  └── LOG resultado
        └── IF rating >= 4.5
              ├── true  → GET Comentarios → LOG Comentarios
              └── false → GET Alternativas (dessert) → LOG Alternativas
```

## Sistema de logging
Cada nodo de código genera un objeto `_log` con:
- `timestamp` — momento exacto de ejecución
- `url` — endpoint consultado
- `status` — SUCCESS o ERROR
- `statusCode` — código HTTP recibido
- `message` — descripción del resultado o error
