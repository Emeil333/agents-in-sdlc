---
title: "Lección 10 - Repaso y pasos siguientes"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI en un flujo de trabajo continuo con Tailspin Toys. En concreto:

- has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y localizado la incidencia de filtrado.
- has añadido valoraciones por estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR).
- has partido de la incidencia de filtrado, acordado el enfoque en modo Plan, lo has implementado en modo Autopilot y lo has revisado en modo Interactive.
- has guiado al agente con instrucciones personalizadas, adaptado la habilidad `quality-checks` y la has utilizado para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has añadido el servidor Playwright Model Context Protocol (MCP) y lo has utilizado para probar el filtrado en un navegador real.
- has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- has revisado todos los cambios del filtrado y autorizado Agent Merge para combinar la PR.
- has explorado comandos de barra para gestionar el contexto y los modelos, compartir sesiones y delegar trabajo en la nube de forma opcional.

## Lo que has publicado

El taller incluye dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizado:

1. **Valoraciones por estrellas:** mostrar el valor existente de `starRating` y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Filtrado y flujo de calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil de QA e incluir las pruebas asociadas.

Desde la planificación del filtrado hasta la apertura de su PR, has usado la misma conversación y rama. Hemos combinado ese trabajo en una PR para simplificar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias maneras: con pruebas automatizadas, revisándolo en el navegador y dejando que Copilot lo explorase mediante MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, el lint y las comprobaciones de tipos, y generó un informe en el nuevo formato. Antes de abrir la PR, QA reunió esos resultados con una revisión de los requisitos y de la cobertura de pruebas.

Las pruebas nuevas deben cubrir carencias reales; si QA no necesita añadir ninguna, el resultado puede ser correcto. Antes de autorizar la combinación, revisa el código y las pruebas, y actualiza las pruebas afectadas después de hacer cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado instrucciones, adaptado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza esas personalizaciones en otras conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para cada tarea. Usa **Plan** para estudiar un enfoque antes de implementarlo, **Interactive** para colaborar en cambios concretos y **Autopilot** para tareas bien acotadas. Elige un modelo más rápido para ediciones rutinarias y otro más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir claramente *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más opciones para explorar

Ya has recorrido el flujo de trabajo principal. Estas funciones de CLI también pueden resultarte útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar subtareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Pasos siguientes

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola: para código de producción, proyectos personales o esa pequeña aplicación que llevas años queriendo crear. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
