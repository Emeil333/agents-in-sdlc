---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y comprobar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica el Space Quiz en una URL pública con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiarte durante la publicación, pero debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure una implementación en GitHub Pages.
- revisarás el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla.
- seguirás la implementación y comprobarás el sitio publicado.

## Escenario

El Space Quiz ya está en un repositorio de GitHub, pero hace falta una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él desde una URL pública sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que publique el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Habilita GitHub Pages para este repositorio. Crea un flujo de trabajo de GitHub Actions que compile el sitio web si es necesario y lo publique en GitHub Pages. Añade un README que describa la aplicación y explique cómo ejecutarla en local si todavía no existe. Crea una solicitud de incorporación de cambios con estos cambios y combínala cuando se superen las comprobaciones obligatorias. Supervisa la implementación hasta que termine correctamente y, después, indícame la URL final del sitio web publicado. Si no tienes permiso para habilitar Pages o combinar la solicitud, dime exactamente qué debo aprobar o hacer; no indiques que ha terminado correctamente hasta que el sitio esté publicado.
```

![Diagrama en tres fases: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y el Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pide al agente que cree y combine una solicitud de incorporación de cambios y cambie la configuración de Pages del repositorio. Revisa las diferencias, los permisos de implementación y cualquier solicitud de aprobación de GitHub antes de continuar.

## Revisa el flujo de trabajo y comprueba la implementación

1. Confirma que el README solo se ha añadido si el repositorio todavía no tenía uno.
2. Revisa los cambios de la solicitud de incorporación de cambios, incluidos los pasos de compilación e implementación del flujo de trabajo.
3. Combínala solo cuando se hayan superado las comprobaciones obligatorias.
4. Sigue la ejecución de implementación de GitHub Actions hasta que termine correctamente.
5. Abre la URL indicada y confirma que se carga el Space Quiz.

Si el agente no puede habilitar Pages o combinar la solicitud, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que se complete la implementación y se cargue el sitio publicado.

## Resumen y pasos siguientes

El Space Quiz ya está disponible como sitio publicado y has comprobado la implementación en lugar de confiar únicamente en el informe del agente. Continúa con la [lección 13: repaso y pasos siguientes][next-lesson].

[next-lesson]: ../13-review/
