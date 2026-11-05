---
title: "Lección 2 - Añadir valoraciones por estrellas: una mejora rápida"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revísalo en un navegador reenviado y combínalo como tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que has instalado Copilot CLI y has probado una conversación, es el momento de hacer el primer cambio en el proyecto. Será algo pequeño: los juegos ya incluyen una valoración por estrellas en sus datos, pero las tarjetas de la página de inicio todavía no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás como tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una conversación de Copilot centrada en una rama de funcionalidad.
- pedirás al agente que haga un pequeño cambio en el proyecto.
- revisarás el cambio con `/diff`.
- ejecutarás la aplicación para confirmar el cambio en un navegador reenviado.
- abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles del juego. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, el editor y la descripción. Como ejercicio inicial, pedirás al agente que muestre en cada tarjeta la valoración existente: un cambio pequeño y autocontenido, perfecto para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI usa el repositorio y la rama de Git que están actualmente seleccionados en el terminal, en lugar de crear un árbol de trabajo específico. Las conversaciones guardadas permiten volver a la misma conversación más tarde, mientras que los archivos y la rama siguen siendo el estado habitual de Git en el disco.

En una conversación verás tres elementos: tus prompts y las respuestas del agente, la actividad de las herramientas mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Iniciar una conversación y solicitar el cambio

Inicia una conversación nueva para implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot no está abierto, inicia Copilot con el comando siguiente:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Usa el prompt siguiente para solicitar el cambio:

   ```plaintext
   Muestra la valoración starRating sobre 5 de cada juego en las tarjetas de la página de lista. Si la valoración es null, muestra "No rating yet". Mantén el diseño de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. ¡Ya has añadido una funcionalidad con Copilot CLI!

## Revisar las diferencias

Todos los cambios generados por IA deben revisarse antes de combinarlos, aunque sean pequeños. Vamos a inspeccionar los cambios directamente en Copilot CLI.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que haya ejecutado Copilot y pídele que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, es probabilístico y no determinista; por eso, el código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación idéntica.

## Comprobar los cambios

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que inicie el sitio para examinar la interfaz actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda revisar en el navegador el cambio de la valoración por estrellas. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran sus valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Detén el servidor de desarrollo que has iniciado.
   ```

## Abrir y combinar tu primera solicitud de incorporación de cambios

¡Ya has creado la funcionalidad! Ahora abrirás una solicitud de incorporación de cambios (PR) para integrarla en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Confirma los cambios revisados de la valoración por estrellas con un mensaje de confirmación adecuado.
   ```

2. Escribe `/pr create`. Copilot CLI enviará la confirmación existente al crear la PR y mostrará la URL.
3. Abre la PR manteniendo pulsado <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y pasos siguientes

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- has iniciado una conversación centrada en una rama de funcionalidad.
- has pedido al agente que haga un pequeño cambio en las tarjetas de juegos.
- has revisado el cambio con `/diff`.
- has ejecutado la aplicación para confirmar la valoración por estrellas en un navegador reenviado.
- has abierto y combinado tu primera solicitud de incorporación de cambios.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
