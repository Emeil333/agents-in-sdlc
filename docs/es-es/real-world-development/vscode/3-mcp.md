---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear el filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. Configurarás el **servidor Playwright MCP** para que Copilot controle un navegador real y pruebe la funcionalidad; después, publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- añadirás el servidor Playwright MCP al espacio de trabajo.
- pedirás a Copilot que pruebe manualmente el filtrado en un navegador.
- publicarás la rama y abrirás una PR.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente es más potente cuando puede acceder a recursos externos al editor. Model Context Protocol (MCP) proporciona un estándar para que el agente se comunique con herramientas y servicios externos.

![Diagrama del modo agente y su interacción con el contexto, el modelo de lenguaje y las herramientas, incluidas las herramientas aportadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Un servidor MCP actúa de puente entre el agente y las herramientas externas, como API existentes o paquetes locales de NPM. Cada servidor ofrece un conjunto diferente de herramientas y recursos.

Algunos servidores MCP populares son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios de GitHub, crear repositorios y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona automatización del navegador con Playwright.

GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar la búsqueda y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Antes de usarlos, revisa su código fuente, comprueba quién los publica y considera las implicaciones de seguridad. Usa solo servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Verás que ya incluye un servidor `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La plantilla ya incluye este servidor MCP remoto de GitHub. Por eso Copilot pudo consultar el backlog en el ejercicio anterior. VS Code se conecta por HTTP y solicita autenticación con GitHub la primera vez que Copilot utiliza una herramienta.

## Añadir el servidor Playwright MCP

El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador controlable para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

El servidor `playwright` es local: VS Code lo inicia mediante `npx @playwright/mcp@latest`. La opción `--headless` ejecuta el navegador sin ventana, algo necesario en un codespace sin escritorio.

> [!NOTE]
> Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador ya estará instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Iniciar y autorizar el servidor

VS Code inicia los servidores MCP cuando se necesitan. La primera vez que Copilot utilice sus herramientas, VS Code iniciará el servidor y te pedirá que confirmes que confías en él.

1. En `.vscode/mcp.json`, selecciona **Start** sobre la entrada `playwright` para iniciar el servidor y comprobar la configuración.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y autorízalo.

Este paso es opcional: si no inicias el servidor manualmente, VS Code lo iniciará cuando Copilot necesite sus herramientas.

## Probar la funcionalidad de filtrado

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que compruebe el filtrado:

```text
Usa el servidor Playwright MCP para abrir un navegador en la aplicación que se ejecuta en http://localhost:4321 y comprobar la nueva funcionalidad de filtrado de juegos:

1. Ve a la página de juegos e indica cuántos aparecen.
2. Aplica un filtro de categoría y confirma que solo aparecen los juegos de esa categoría.
3. Quítalo, aplica un filtro de editorial y confirma que la lista se actualiza.
4. Combina un filtro de categoría y otro de editorial y confirma que los resultados cumplen ambos.

Describe lo que observas en cada paso e indica cualquier comportamiento inesperado.
```

Copilot abrirá un navegador mediante Playwright MCP, seguirá los pasos y resumirá los resultados. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona, pide aclaraciones o solicita que corrija el código antes de abrir la PR.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321`. Si detuviste el servidor de desarrollo, inícialo de nuevo. La primera vez, Copilot quizá necesite descargar un navegador; si indica que falta, pídele que ejecute `npx playwright install chromium`.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de incorporación de cambios

1. Abre el panel **Source Control** del Codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear la solicitud de incorporación de cambios

Copilot puede localizar la incidencia correspondiente y crear la PR.

1. En Copilot Chat, selecciona **New Chat**.
2. Confirma que está seleccionado **Agent** para que Copilot pueda usar las herramientas de GitHub.
3. Envía el prompt:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con el filtrado por categoría y editorial. Crea una solicitud de incorporación de cambios para la rama actual y relaciónala con la incidencia correcta.
   ```

4. Cuando sea necesario, selecciona **Continue** para autorizar las acciones. La primera vez que se use una herramienta de GitHub, quizá debas autenticarte.
5. Comprueba que Copilot localiza la incidencia correcta y crea la PR.
6. Abre el enlace y revisa la solicitud, pero **no la combines todavía**.

## Resumen y pasos siguientes

Has aprendido qué es MCP, añadido Playwright MCP, utilizado un navegador para probar el filtrado y publicado la rama en una PR. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor Microsoft Playwright MCP][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
