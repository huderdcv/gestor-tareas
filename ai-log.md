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

### Entrada 2

- **Fecha:** 2026-11-24
- **Paso del laboratorio:** plan (TDD de `completar`, `eliminar` y `editar`)
- **Prompt clave (literal):**
  > Quiero implementar completar, eliminar y editar de tareas.py con TDD, una función
  > a la vez. Propón un plan: para cada función, qué tests escribirías (uno por cada
  > regla del docstring) y en qué orden. No escribas código todavía.
- **Qué propuso el agente:** implementar en orden `completar` → `eliminar` → `editar`,
  con un ciclo RED → GREEN → verificación → commit por función, 15 tests nuevos
  (4, 4 y 7, uno por regla del docstring) y un REFACTOR final que extrae la validación
  de título para que la compartan `agregar` y `editar`.
- **Decisión:** aceptado sin cambios. El orden va de menor a mayor complejidad, cada
  función termina en un commit propio y el refactor queda separado para no mezclarlo
  con la implementación de `editar`. Crítica del plan:
  - ¿Cubre cada regla del docstring? Sí: hay pruebas para cada regla de cada función.
  - ¿Empieza por los tests? Sí: los tests son la fase inicial de cada función.
  - ¿Propone tocar `servidor.py` o agregar dependencias? No: el plan indica que no toca
    esos archivos ni agrega dependencias.
- **Cómo lo verifiqué:** leí el plan completo y lo comparé con las reglas de los
  docstrings de `tareas.py`; no se ejecutó código.

### Entrada 3

- **Fecha:** 2026-11-24
- **Paso del laboratorio:** RED de `completar`
- **Prompt clave (literal):**
  > Escribe en `test_tareas.py` los tests de `completar`, uno por cada regla de su
  > docstring, con el mismo estilo que los tests de `agregar`. No modifiques `tareas.py`.
- **Qué propuso el agente:** 4 tests en `test_tareas.py` con el estilo de los de
  `agregar`: marca y devuelve la tarea sin alterar las demás, idempotencia sobre una
  tarea ya hecha, id inexistente con el mensaje exacto y lista intacta, y mensaje con el
  id pedido. También amplió el import a `from tareas import agregar, completar`.
- **Decisión:** aceptado sin cambios. Los 4 tests cubren una a una las reglas del
  docstring, respetan el estilo existente y no tocan `tareas.py`.
- **Cómo lo verifiqué:** el agente ejecutó `python -m pytest` al terminar los tests →
  4 failed con `NotImplementedError` y 8 passed; después ejecuté yo el comando
  manualmente y obtuve el mismo resultado: es el RED esperado y ningún test nuevo pasa
  por accidente.

### Entrada 4

- **Fecha:** 2026-11-24
- **Paso del laboratorio:** GREEN de `completar`
- **Prompt clave (literal):**
  > Implementa completar en tareas.py para que pasen los tests. No modifiques los tests.
- **Qué propuso el agente:** recorrer la lista, marcar `hecha = True` y devolver la misma
  tarea; si no encuentra el id, lanzar `ValueError(f"No existe la tarea {id_tarea}")` con
  el id pedido. No tocó los tests.
- **Decisión:** aceptado sin cambios. Es la implementación mínima que cumple las dos
  reglas del docstring, mantiene el estilo de `agregar()` y no agrega dependencias.
- **Cómo lo verifiqué:** el agente ejecutó `python -m pytest` → 12 passed; después
  ejecuté yo el comando manualmente y obtuve el mismo resultado; revisé `git diff` y solo
  cambió el cuerpo de `completar()`; recargué la página con `python servidor.py` y al
  pulsar **Completar** la tarea aparece tachada en verde.

### Entrada 5

- **Fecha:** 2026-11-25
- **Paso del laboratorio:** deshacer (paso 5)
- **Prompt clave (literal):**
  > Traduce al inglés todos los mensajes de error de tareas.py
- **Qué propuso el agente:** tradujo al inglés los `ValueError` de las funciones ya
  implementadas y ejecutó los tests, que fallaron porque comparan los mensajes exactos
  en español.
- **Decisión:** rechazado. Los mensajes de error son el contrato que comparan los tests;
  el cambio se deshizo con `/undo`.
- **Cómo lo verifiqué:** revisé los mensajes de `tareas.py` y `git diff`, ejecuté `/undo`
  y comprobé con `git status` que `tareas.py` ya no aparecía como modificado.

### Entrada 6

- **Fecha:** 2026-11-25
- **Paso del laboratorio:** RED de `eliminar`
- **Prompt clave (literal):**
  > implementa los test para la funcion eliminar en test_tareas.py. que tengan el mismo
  > estilo de los tests ya implementados.
- **Qué propuso el agente:** 4 tests con el estilo existente: quita la tarea y la
  devuelve, los ids de las demás no cambian, id inexistente con el mensaje exacto y lista
  intacta, y mensaje con el id pedido. Amplió el import a
  `from tareas import agregar, completar, eliminar`.
- **Decisión:** aceptado sin cambios. Un test por regla del docstring, mismo estilo y sin
  tocar `tareas.py`.
- **Cómo lo verifiqué:** el agente ejecutó `python -m pytest` → 4 failed con
  `NotImplementedError` y 12 passed; después ejecuté yo el comando manualmente y obtuve
  el mismo resultado; revisé los tests y los comparé con las reglas del docstring, y
  comprobé que solo se modificó `test_tareas.py`, ningún otro archivo.

### Entrada 7

- **Fecha:** 2026-11-25
- **Paso del laboratorio:** GREEN de `eliminar`
- **Prompt clave (literal):**
  > implementa la funcion eliminar en tareas.py para que sus respectivos tests pasen. no
  > modifiques los tests
- **Qué propuso el agente:** recorrer la lista con `enumerate`, devolver
  `tareas.pop(indice)` al encontrar el id y, si no existe, lanzar
  `ValueError(f"No existe la tarea {id_tarea}")`. Al no reasignar ids, las demás tareas
  los conservan.
- **Decisión:** aceptado sin cambios. Implementación mínima que cumple las dos reglas del
  docstring, con el mismo estilo que `completar()`.
- **Cómo lo verifiqué:** el agente ejecutó `python -m pytest` → 16 passed; después
  ejecuté yo el comando manualmente y obtuve el mismo resultado; revisé el cambio y
  comprobé que no se modificaron otros archivos; verifiqué la funcionalidad en el
  navegador con `python servidor.py` y el botón **Eliminar**.

### Entrada 8

- **Fecha:** 2026-11-25
- **Paso del laboratorio:** RED de `editar`
- **Prompt clave (literal):**
  > implementar los tests de tu plan para la funcion de editar. que sigan el mismo estilo
  > de los tests ya implementados
- **Qué propuso el agente:** 7 tests con el estilo existente: cambia el título y devuelve
  la tarea, quita espacios de los extremos, título vacío, límite de 80 caracteres, 81
  caracteres, id inexistente y `hecha` conservada. Amplió el import a
  `from tareas import agregar, completar, editar, eliminar`. Único ajuste respecto al
  plan: en vez de `parametrize`, el test de `hecha` comprueba `True` y `False` con dos
  `assert`, para no introducir un patrón ausente en el resto de los tests.
- **Decisión:** aceptado sin cambios. Un test por regla del docstring, mismo estilo y sin
  tocar `tareas.py`.
- **Cómo lo verifiqué:** el agente ejecutó `python -m pytest` → 7 failed con
  `NotImplementedError` y 16 passed; después ejecuté yo el comando manualmente y obtuve
  el mismo resultado; revisé los tests y los comparé con las reglas del docstring, y
  comprobé que solo se modificó `test_tareas.py`.

### Entrada 9

- **Fecha:** 2026-11-25
- **Paso del laboratorio:** GREEN de `editar`
- **Prompt clave (literal):**
  > vamos por la fase green. implementa la funcion editar en tareas.py con lo minimo
  > necesario para que sus respectivas pruebas pasen. no toques los tests.
- **Qué propuso el agente:** validar el título con las mismas tres reglas de `agregar()`
  (duplicadas a propósito, por ser lo mínimo), buscar la tarea por id, asignar el título
  limpio y devolverla; si no existe, `ValueError(f"No existe la tarea {id_tarea}")`. No
  modificó `hecha` ni los tests.
- **Decisión:** aceptado sin cambios. La duplicación de la validación queda para el paso
  REFACTOR.
- **Cómo lo verifiqué:** el agente ejecutó `python -m pytest` → 23 passed; después ejecuté
  yo el comando manualmente y obtuve el mismo resultado; revisé el cambio y comprobé que
  solo se modificó `tareas.py`.

### Entrada 10

- **Fecha:** 2026-11-25
- **Paso del laboratorio:** refactor (validación de título compartida)
- **Prompt clave (literal):**
  > fase refactor: refactoriza la funcion de editar para extraer en una sola funcion las
  > reglas de titulo que comparte con la funcion de agregar
- **Qué propuso el agente:** extraer `validar_titulo(titulo)`, que aplica `strip`, rechaza
  el título vacío y el de más de 80 caracteres, y devuelve el título limpio; `agregar()` y
  `editar()` la usan en lugar de repetir las reglas.
- **Decisión:** aceptado sin cambios. La función devuelve el título limpio porque ambos
  llamadores necesitan ese valor, y conserva los mensajes exactos que comparan los tests.
- **Cómo lo verifiqué:** el agente ejecutó `python -m pytest` → 23 passed; después ejecuté
  yo el comando manualmente y obtuve el mismo resultado; revisé `git diff` y solo cambió
  `tareas.py`, sin tocar los tests ni los mensajes de error; verifiqué en el navegador el
  funcionamiento de cada función refactorizada, **Agregar** y **Editar**.
