---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para desarrollar el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos por añadir una pequeña funcionalidad al proyecto. Sin embargo, los cambios más amplios requieren un proceso más robusto. GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de una organización y ayudarnos a crear lo adecuado de la manera adecuada. En esta y varias lecciones siguientes seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás el código, validarás el comportamiento y, finalmente, combinarás los cambios.

> [!NOTE]
> A medida que desarrolles esta funcionalidad, seguirás usando la misma conversación y rama. Normalmente se usarían ramas o PR distintas para los diferentes tipos de archivo, pero en este taller simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot a partir de una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás manualmente la funcionalidad en un navegador reenviado.

Durante el desarrollo actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero es necesario acordar algunos detalles, como la combinación de categorías, antes de programarla. Utilizarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA a tu flujo de desarrollo no cambia sus fundamentos; de hecho, los hace aún más importantes. El proceso de la mayoría de los desarrolladores se parece a este:

1. Empezar por una incidencia registrada que describa el trabajo.
2. Planificar lo que se va a crear.
3. Programar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código cuando se haya revisado y se hayan superado las comprobaciones de integración continua.

> [!NOTE]
> Los detalles concretos dependen del equipo y de la organización, pero la mayoría de los procesos son variaciones de este flujo.

Si sigues este enfoque estándar, te aseguras de que el código generado por IA cumple los requisitos y pasa por el mismo proceso de validación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: trabajas con el agente. Este propone cambios y espera tus indicaciones para continuar.
- **Plan**: el agente crea primero un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empezar desde la incidencia

Antes de trabajar en el filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal estén preparados.

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que se ha combinado la PR de valoración por estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza con `#7` (o un número similar). El carácter `#` permite añadir al contexto una incidencia o una solicitud de incorporación de cambios de GitHub.

## Planificar la funcionalidad de filtrado

La planificación permite acordar cómo implementar una funcionalidad o realizar una tarea antes de delegarla a Copilot. Es recomendable dedicar tiempo a planificar las tareas complejas. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe el prompt siguiente:

   ```plaintext
   Crea un plan para implementar esta funcionalidad.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque.

3. Es posible que Copilot te pregunte cómo debe funcionar el filtrado. Responde según tus preferencias; no hay una única respuesta correcta.
4. Cuando el plan esté completo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que modifique las partes que no coincidan con tus decisiones.

## Aprobar Autopilot

Con el plan escrito y revisado, es el momento de implementarlo. Deja que Copilot trabaje en la tarea con Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que lo ha completado.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción con un nombre parecido en tu versión.
2. Confirma que el indicador debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente, así que revísalo antes. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas.

## Revisar y comprobar la implementación

Como con cualquier otro código, hay que revisar el código generado antes de combinarlo. Revisa los cambios y ejecuta el sitio para comprobar el resultado.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones de planificación.
4. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los errores.

## Explorar la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda probar la funcionalidad de filtrado en el navegador. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, por editorial y las combinaciones acordadas en el plan.
4. Confirma que el reinicio de los filtros y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo.

## Resumen y pasos siguientes

Has utilizado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot a partir de una incidencia de GitHub.
- has definido los requisitos en el modo Plan.
- has implementado la funcionalidad con el modo Autopilot.
- has revisado el código.
- has validado manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en cómo se genera el código y cómo garantizar que siga las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
