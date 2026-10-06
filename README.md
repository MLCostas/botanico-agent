# Botanico Agent 🌿

Agente conversacional en Python que responde preguntas sobre flora del NEA argentino usando **herramientas (tool use)** sobre un dataset propio. Nació como prototipo de la arquitectura de un agente IA para chatbots: el LLM decide qué herramienta llamar, el código la ejecuta y el resultado vuelve al modelo.

## Qué demuestra
- **Arquitectura de agente**: bucle de tool use (modelo → herramienta → resultado → modelo) con límite de pasos.
- **Backends intercambiables**: `ReglasBackend` (offline, determinista, usado en los tests) y `AnthropicBackend` (LLM real). Cambiar de proveedor implica escribir una sola clase.
- **Herramientas con esquema JSON**: `buscar_especie`, `listar_familia`, `estadisticas` (pandas), con búsqueda insensible a acentos y mayúsculas.
- **Manejo de errores**: herramientas desconocidas o argumentos inválidos devuelven un error estructurado en lugar de romper el agente; si la especie no está en la base, el agente lo dice y no inventa.
- **Tests automatizados** con pytest.

## Estructura
```
src/botanico/
  agent.py      # Agente: historial + conexión backend/herramientas
  backends.py   # ReglasBackend y AnthropicBackend
  tools.py      # herramientas y esquemas
  cli.py        # chat por consola
data/flora_nea.csv
tests/test_agent.py
```

## Uso
```bash
pip install -r requirements.txt
PYTHONPATH=src python -m botanico.cli          # modo offline
pip install anthropic
export ANTHROPIC_API_KEY=...
PYTHONPATH=src python -m botanico.cli --llm    # modo LLM
pytest
```

Ejemplo:
```
vos> hablame del ceibo
bot> Ceibo (Erythrina crista-galli), familia Fabaceae. Habito: arbol. Region: Litoral y Paraguay-Parana. Flor nacional argentina.
```

## Limitaciones y próximos pasos
- El dataset es una muestra de 12 especies; el objetivo es la arquitectura, no la cobertura. Los datos deben verificarse antes de usarse con fines científicos.
- Memoria de conversación solo en sesión; falta persistencia.
- Próximos pasos: integrar un modelo de visión para identificar especies desde fotos, exponer el agente por API (FastAPI) o conectarlo a WhatsApp/Telegram, y agregar evaluación de respuestas.

## Autora
María Luisa Costas — Estudiante de Cs. Biológicas (FaCENA-UNNE) y Tec. en Ciencia de Datos (UNDEC).
