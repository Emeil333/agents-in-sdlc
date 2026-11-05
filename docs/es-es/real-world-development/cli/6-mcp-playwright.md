---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar en el navegador la funcionalidad de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, programar no consiste únicamente en escribir código. También hay que trabajar con datos y servicios externos, y proporcionar a Copilot automatizaciones adicionales. Para ello sirven los servidores MCP: permiten que Copilot vaya más allá de las funciones integradas en la CLI y acceda a herramientas y servicios adicionales.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadirás el servidor Playwright MCP si todavía no está disponible.
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz hay que interactuar con ella. Quieres que Copilot pueda utilizar el sitio web como lo haría una persona y automatizar así la comprobación de los cambios.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares son:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: permite automatizar el navegador con Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] que facilita descubrir servidores y contribuir al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad.

## Añadir el servidor Playwright MCP

Si el servidor Playwright MCP todavía no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, escribe el comando siguiente para abrir el cuadro de diálogo para añadir servidores MCP:

   ```plaintext
   /mcp add
   ```

3. Como nombre, escribe `playwright` y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** con <kbd>Enter</kbd> y, a continuación, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo.

## Pedir a Copilot que explore la funcionalidad con Playwright

Antes confirmaste manualmente que la funcionalidad se comporta como esperabas. Ahora usarás el servidor Playwright MCP disponible en Copilot para repetir la comprobación.

1. Usa el prompt siguiente para pedir a Copilot que valide la funcionalidad con Playwright MCP:

   ```plaintext
   Inicia la aplicación y usa Playwright MCP para comprobar el filtrado según la incidencia y el plan. Dime qué funciona y qué no, sin hacer cambios. Detén el servidor que hayas iniciado cuando termines.
   ```

> [!NOTE]
> Normalmente no es necesario indicar que use el servidor MCP: Copilot suele identificarlo por sí mismo. Aun así, señalar la herramienta puede hacer que los resultados sean más coherentes y ahorrar tokens.

2. Observa los pasos que ejecuta Copilot en el navegador para comprobar la funcionalidad.
3. Lee el informe y confirma que todo se comporta como se esperaba.

Copilot iniciará el servidor, interactuará con el sitio mediante Playwright, detendrá el servidor y te mostrará un informe.

## Resumen y pasos siguientes

¡Enhorabuena! Has usado el servidor Playwright MCP para explorar la funcionalidad en un navegador real desde Copilot CLI. En esta lección:

- has aprendido qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- has añadido el servidor Playwright MCP si todavía no estaba disponible.
- has pedido al agente que controle un navegador y explore la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador en un perfil especializado.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor Microsoft Playwright MCP][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
