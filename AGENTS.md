# AGENTS.md

Laboratorio de TDD sobre un gestor de tareas en Python. `README.md` es la guía del
laboratorio: sus restricciones mandan sobre cualquier otra convención.
Todo el proyecto (docstrings, tests, mensajes de error, nombres) está en español:
mantén el español, aunque el resto de tus convenciones sea escribir código en inglés.

## Comandos

- `python -m pytest` — suite completa (los 8 tests de `agregar` deben seguir en verde).
- `python -m pytest test_tareas.py::test_agregar_titulo_vacio_da_error` — un solo test.
- `python servidor.py` — vista web en http://127.0.0.1:8000; `python servidor.py 8001`
  si el puerto está ocupado. No hace falta reiniciar: el servidor reimporta `tareas.py`
  en cada petición, así que basta recargar la página.

## Flujo obligatorio (TDD, una función a la vez)

Por cada función de `tareas.py`, en este orden:
tests que fallan con `NotImplementedError` (RED) → implementación mínima (GREEN) →
refactor → commit. No implementes `completar`, `eliminar` ni `editar` antes de tener
sus tests escritos y fallando.

## Reglas

- No modifiques `servidor.py` ni `almacen.py`: vienen terminados.
- Solo biblioteca estándar; `pytest` únicamente para los tests.
- No agregues dependencias.
- Los mensajes de `ValueError` deben ser exactamente los de los docstrings: son el
  contrato que comparan los tests.
- Nunca cambies un test para que pase. Si falla, corrige `tareas.py`.
- `editar()` comparte las reglas de título con `agregar()`: una sola función de
  validación para ambas (paso REFACTOR).
- `tareas.json` es estado de ejecución y está en `.gitignore`: no lo edites ni lo commitees.
- Cuando te lo indique explicitamente me presentarás un plan para agregar una entrada en el archivo `ai-log.md` con los nuevos cambios, interacciónes, decisiones que se tomaron. Para la presentación de este plan debes de seguir las reglas que se mencionan en `ai-log.md`. Me preguntaras, sesde que interacción se considera la entrada, que acepte, rechaze, ajuste, etc. todo lo necesario para tener una entrada bien documentada. Antes de agregar la entrada presentado en el plan yo debo aceptarlo explicitamente.
- Cuando te lo indique explicitamente crearas un commit con para los ultimos cambios de todos archivos, pero antes de realizar el commit me daras opciones para el nombre.
