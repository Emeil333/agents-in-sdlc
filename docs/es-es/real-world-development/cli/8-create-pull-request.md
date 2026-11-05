---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad actualizada, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Combinaste personalmente la PR de valoración por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> En condiciones normales, dividiríamos la funcionalidad, las instrucciones, la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el trabajo de filtrado y del flujo de calidad en una conversación y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionarás todos los cambios de la funcionalidad y las pruebas de verificación.
- crearás la PR del filtrado.
- activarás Agent Merge solo después de la revisión y confirmarás que se ha combinado la PR.

## Escenario

Durante el flujo de trabajo de filtrado has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR sin renunciar al control del desarrollador sobre la autorización de la combinación.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Al habilitarlo, Copilot resuelve los bloqueos de la PR, como las comprobaciones de integración continua (CI) fallidas, los comentarios de revisión y los cambios que requieren una actualización de la rama; después, habilita la combinación automática de GitHub cuando la configuración del repositorio lo permite.

Hasta ahora has seleccionado personalmente **Merge pull request**. Agent Merge puede encargarse de esa tarea. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usar Agent Merge para gestionar la PR

Con el código terminado, revisa los cambios, crea la PR y deja que Agent Merge se encargue del proceso restante.

1. Vuelve a tu Codespace.
2. Escribe `/agent` para abrir el cuadro de diálogo de agentes.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todas las comprobaciones terminen correctamente, realizará la combinación.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, revisa el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y gestionar la solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- has inspeccionado todos los cambios de la funcionalidad y las pruebas de verificación.
- has creado la PR del filtrado.
- has activado Agent Merge después de la revisión y confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto y los modelos, compartir sesiones y delegar trabajo en la nube de forma opcional.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
