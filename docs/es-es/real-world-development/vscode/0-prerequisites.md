---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara el entorno. Crearás una copia propia del repositorio Tailspin Toys, iniciarás un [codespace][codespaces] y confirmarás que GitHub Copilot Chat funciona en el editor.

## Configurar el repositorio del laboratorio

Para crear una copia del repositorio en la que trabajarás, utiliza la [plantilla][template-repository].

1. En una nueva ventana del navegador, ve a la [plantilla de Tailspin Toys][tailspin-template].
2. Crea una copia propia del repositorio seleccionando **Use this template** en la página del repositorio del laboratorio. Después, selecciona **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento dirigido por GitHub o Microsoft, sigue las instrucciones de los mentores. De lo contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Configuración del repositorio de plantilla](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**), ya que la necesitarás más adelante.

> [!NOTE]
> **El backlog ya está preparado**
>
> Al crear el repositorio desde la plantilla, se generará automáticamente un backlog de incidencias de GitHub. Trabajarás con estas incidencias durante el taller; no tienes que crear ninguna.

## Crear un codespace

A continuación, usarás un codespace para completar los ejercicios.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un IDE completo con compatibilidad con varios lenguajes de programación, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Seleccionar el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un codespace.

    ![Crear un codespace](../../../_images/ex0-create-codespace.png)

La creación del codespace tardará unos minutos, aunque será mucho más rápida que instalar todos los servicios manualmente. Mientras tanto, puedes explorar otras funciones de GitHub Copilot.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está diseñado para ejecutarse en un codespace o en un [contenedor de desarrollo local][dev-containers]. Ambos entornos incluyen los requisitos necesarios. Si prefieres trabajar en local, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor de desarrollo que utiliza el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usar GitHub Copilot Chat y el modo agente

Para acceder al modo agente de GitHub Copilot Chat, necesitas tener instalada la extensión GitHub Copilot Chat en el IDE. Si utilizas un GitHub Codespace, ya debería estar instalada.

> [!TIP]
> Si todavía no tienes la extensión, puedes [instalarla desde Visual Studio Code Marketplace][copilot-chat-extension]. También puedes abrir la vista **Extensions** en Visual Studio Code, buscar **GitHub Copilot Chat** y seleccionar **Install**.

Cuando la extensión esté instalada, quizá tengas que autenticarte con tu cuenta de GitHub.

1. Vuelve a tu codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona el icono **Copilot Chat** en la parte superior del codespace.
3. Escribe algo como «Hola» en la ventana de Copilot Chat y pulsa <kbd>Enter</kbd>. Esto iniciará Copilot Chat.
4. Si todavía no te has autenticado, se te pedirá que inicies sesión en GitHub. Sigue las instrucciones para autenticarte.

    ![Ejemplo del mensaje de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, aparecerá la ventana de Copilot Chat.

## Resumen

Enhorabuena: ya tienes una copia del repositorio y has empezado a crear el codespace que usarás para programar.

## Siguiente paso

Empezarás a trabajar con Copilot en el [ejercicio 1: instrucciones personalizadas][next-lesson], donde aprenderás a indicarle las convenciones del proyecto.

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Introducción a Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
