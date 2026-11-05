---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores pediste al agente de Copilot en la nube que realizara tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque trabaja de forma autónoma y asíncrona, es importante supervisar las tareas.

Hay varias herramientas para gestionar el trabajo asignado al agente, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas, consultar las acciones realizadas y dirigir las sesiones que siguen en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas.
- dirigirás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo decidió que también sería un buen momento para añadir un modo claro. Como ya se estaban actualizando los estilos y añadiendo conmutadores, conviene incluir esta opción. Dirigirás el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisar las tareas del agente de Copilot en la nube

Consulta el estado de las tareas que has asignado al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa las tareas del panel principal y del panel izquierdo. Deberías ver las tareas asignadas, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las acciones que ha realizado Copilot. Pueden incluir:
   - obtener una copia del código del repositorio.
   - preparar el entorno de trabajo.
   - configurar servidores MCP.
   - ejecutar los pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado de la tarea y el enfoque de Copilot.

4. Observa el panel de PR, que aparece a la derecha y permite consultar la solicitud y los archivos modificados.

## Dirigir el agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título puede variar según el nombre que haya elegido Copilot y el estado de la tarea.

   ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que la sesión indique que terminó la configuración y empezó a trabajar. Verás mensajes similares a los que se muestran a continuación.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   Ya que estamos trabajando en un modo de alto contraste, añade también un modo claro. Debe haber un conmutador para que los usuarios puedan seleccionar el modo de visualización que prefieran.
   ```

   ![Tarea del agente en la página de agentes con el cuadro de diálogo para dirigir una sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma la solicitud y la incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Copilot incorporará la nueva solicitud cuando termine el paso en el que estaba trabajando al recibir tu mensaje.

Como antes, el proceso tardará varios minutos. Puedes aprovechar ese tiempo para repasar lo que has aprendido.

## Resumen y pasos siguientes

Has explorado la página de agentes de Copilot, el centro de control para supervisar y dirigir las tareas del agente en la nube. Permite hacer seguimiento de tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar su alcance o añadir indicaciones.

Has aprendido a usar Mission Control y la página de agentes para supervisar tareas y dirigir una sesión en curso hacia una funcionalidad adicional. Mientras Copilot termina los cambios de accesibilidad, pasarás a [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
