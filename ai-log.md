# Bitácora de IA

Esta bitácora registra cómo trabajaste con el agente durante el laboratorio: qué le pediste, qué propuso, qué decidiste y cómo lo comprobaste. No se evalúa cuánto código escribió la IA, sino tu criterio: por qué aceptaste, rechazaste o ajustaste cada propuesta. Agrega una entrada por cada paso del laboratorio y otra cada vez que deshagas un cambio del agente.

## Plantilla

Copia este bloque para cada entrada nueva:

```markdown
### Entrada N

- **Fecha:** AAAA-MM-DD
- **Paso del laboratorio:** (por ejemplo: memoria del proyecto, plan, RED de completar, GREEN de completar, refactor)
- **Prompt clave (literal):**
  > Pega aquí el prompt exacto que usaste.
- **Qué propuso el agente:** resumen en una o dos líneas.
- **Decisión:** aceptado / rechazado / ajustado. Por qué: ...
- **Cómo lo verifiqué:** tests (`python -m pytest`), navegador (`python servidor.py`), lectura del cambio...
```

## Ejemplo

### Ejemplo de entrada

- **Fecha:** el día del laboratorio
- **Paso del laboratorio:** memoria del proyecto (`/init`)
- **Prompt clave (literal):**
  > /init
- **Qué propuso el agente:** creó `AGENTS.md` con un resumen del proyecto, cómo ejecutarlo y una descripción de los archivos.
- **Decisión:** ajustado. Quité la descripción archivo por archivo, porque el agente la puede deducir leyendo el código. Agregué lo que no puede adivinar: que `servidor.py` y `almacen.py` no se modifican, que los mensajes de error deben ser exactamente los del docstring y que nunca se cambia un test solo para que pase.
- **Cómo lo verifiqué:** leí el archivo completo antes de guardarlo y comprobé que los comandos que menciona funcionan: `python -m pytest` pasa los tests de `agregar` y `python servidor.py` abre la página.

### Entrada 1

- **Fecha:** 2026-11-24
- **Paso del laboratorio:** memoria del proyecto (`AGENTS.md`)
- **Prompt clave (literal):**
  > /init
- **Qué propuso el agente:** un `AGENTS.md` compacto con los comandos, el flujo TDD
  obligatorio y las reglas del laboratorio, sin descripción archivo por archivo ni
  consejos genéricos.
- **Decisión:** ajustado. Acepté la estructura (comandos, TDD, reglas). La prohibición de dependencias se mencionaba en el ítem de pruebas, así que se
  pasó a un ítem nuevo que prohíbe insertar dependencias nuevas en cualquier momento y
  en cualquier parte del sistema. Como el proyecto exige documentar cada decisión en
  `ai-log.md`, se agregó una instrucción para que, cuando se indique explícitamente, el
  agente presente un plan para agregar la entrada y realizar el commit.
- **Cómo lo verifiqué:** `python -m pytest` → 8 passed; el comando de un solo test
  `python -m pytest test_tareas.py::test_agregar_titulo_vacio_da_error` → 1 passed;
  lectura completa de `AGENTS.md` antes de guardarlo.
