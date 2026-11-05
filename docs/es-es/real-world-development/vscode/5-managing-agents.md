---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En el ejercicio anterior pediste al agente de Copilot en modo agente que implementara el modo de alto contraste. Como el trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra las llamadas a herramientas y las ediciones de archivos. Puedes revisar las diferencias, aceptar o rechazar cambios y dirigir la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- pedirás al agente que añada un conmutador de modo claro además del modo de alto contraste.

## Escenario

Mientras el agente personalizado de accesibilidad implementa el modo de alto contraste, el equipo de diseño solicita además un conmutador de modo claro para los usuarios que encuentran demasiado oscuro el tema predeterminado. En vez de empezar desde cero, dirigirás la sesión existente para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio anterior.
2. Mientras trabaja el agente, desplázate por el chat para ver las llamadas a herramientas, las lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando proponga una edición, aparecerá una diferencia integrada en el chat:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y pedir al agente que vuelva a intentarlo.
4. Abre el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta necesita permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará la opción **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Mostrará un mensaje final con un resumen de los cambios.

## Pedir al agente que añada el modo claro

Ahora que ya está implementado el modo de alto contraste, ampliarás la misma conversación para añadir un conmutador de modo claro. Como continuarás en el mismo chat, el agente conservará el contexto del trabajo anterior, incluidos los componentes, los estilos y el patrón de almacenamiento local que utilizó.

1. Envía el siguiente prompt de seguimiento desde Copilot Chat:

   ```plaintext
   Buen trabajo. Ahora añade un conmutador de modo claro junto al de alto contraste. El modo claro debe seguir el mismo patrón: conservarse mediante el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el modo claro y el modo de alto contraste de forma independiente.
   ```

2. El agente continuará desde donde se quedó, revisará los cambios y propondrá los necesarios para el modo claro.
3. Revisa cada edición y usa **Keep** o **Undo**, como hiciste con el modo de alto contraste.
4. Si el agente toma una dirección que no quieres —por ejemplo, intenta reorganizar los conmutadores de una forma que no te convence—, detenlo con **Stop**, junto al cuadro de entrada, y envía un prompt para aclararlo.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar otra. Usa prompts de seguimiento para pedir ajustes, corregir el enfoque o ampliar el trabajo. Por ejemplo: «Simplifica el CSS y usa variables CSS en lugar de duplicar selectores», «Integra el modo claro en el panel de configuración existente» o «Añade atajos de teclado para ambos conmutadores».

## Revisar los cambios

Antes de confirmar el trabajo, revisa los archivos modificados.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos cambiados. Deberías ver componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se aplican los patrones de accesibilidad del agente: atributos ARIA, navegación por teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios en el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y pasos siguientes

Has utilizado Copilot Chat en modo agente para supervisar y dirigir una sesión en curso: has revisado las ediciones, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has aprendido a supervisar el trabajo de un agente en tiempo real, aceptar o rechazar ediciones individuales y dirigir una sesión con prompts de seguimiento. En la [siguiente lección][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios del agente local.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
