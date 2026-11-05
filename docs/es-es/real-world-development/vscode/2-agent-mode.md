---
title: "Ejercicio 2 - Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios sencillos suelen implicar editar varios archivos y ejecutar tareas como las pruebas. Un desarrollador localiza los archivos necesarios, hace los cambios, ejecuta las pruebas, depura y corrige lo que falta. El modo agente de Copilot puede explorar el proyecto, actualizar varios archivos, ejecutar tareas y corregir los errores que encuentre. En este ejercicio lo utilizarás para añadir una funcionalidad.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, con ciclos rápidos de comentarios y revisión. El agente en la nube trabaja de forma asíncrona con incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo el modo agente explora el proyecto, identifica los archivos relevantes, implementa una funcionalidad en la interfaz y la capa de datos, y permite revisar los cambios y las pruebas antes de combinarlos.

## Escenario

El catálogo de juegos está creciendo y quieres que los usuarios puedan filtrar por categoría y editorial. En el ejercicio anterior añadiste un helper de editoriales; ahora completarás la capa de datos, la interfaz y las pruebas con el modo agente.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio para entender su funcionalidad. Tailspin Toys es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Es una aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API independiente ni un framework de interfaz del lado del cliente. Los helpers de acceso a datos se encuentran en `src/lib/` y la interactividad se implementa con scripts Astro acotados que usan las API DOM estándar.

### Iniciar el sitio web

El comando `npm run dev` inicia el servidor de desarrollo de Astro.

1. Vuelve a tu Codespace. Al terminar el ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando se inicie el servidor Astro, verás una dirección similar a `http://localhost:4321`.

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo con **x**.

5. Abre el sitio manteniendo pulsado <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor.

> [!NOTE]
> En Codespaces, al seleccionar el enlace de localhost se te redirige a una URL privada del codespace que aloja el servidor.

### Explorar el sitio

La página de inicio muestra una lista de juegos con sus títulos, imágenes y descripciones. Al seleccionar un juego, se abre su página de detalles con información como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Vuelve al Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo se indican en cada paso.

La aplicación funciona, pero queremos añadir capacidades. Pide a Copilot que muestre las incidencias del backlog.

1. Selecciona **Agent** en la lista de agentes de Chat para permitir que planifique y modifique archivos, ejecute comandos y utilice herramientas.
2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores no recomiendan un modelo concreto. Las instrucciones usan Claude Sonnet 4.5, que se utilizó al crear el taller, para intentar que las sugerencias sean coherentes. Como los modelos de lenguaje son probabilísticos, es normal que los resultados varíen.

> [!NOTE]
> Copilot puede usar un comando MCP distinto y completar la tarea correctamente.

3. Envía este prompt:

   ```plaintext
   Muéstrame el backlog de incidencias del repositorio de GitHub. Ayúdame a priorizarlas según su utilidad para los usuarios.
   ```

4. Selecciona **Continue** para permitir que Copilot consulte las incidencias.
5. Revisa la lista y la priorización.

## Revisar los archivos de instrucciones

Antes de pedir a Copilot que genere código, revisa `.github/instructions/ui.instructions.md`, que describe cómo añadir funcionalidades al sitio.

1. Abre `.github/instructions/ui.instructions.md` en el Codespace.
2. Revisa la arquitectura, los principios de diseño de componentes, las pruebas y la accesibilidad, además de los enlaces a las instrucciones de Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar otros archivos y ejemplos del proyecto. Las rutas son relativas al archivo de instrucciones.

## Implementar el filtrado

Para completar el filtrado, Copilot deberá actualizar la lógica de `src/lib/`, añadir o modificar pruebas y actualizar la página de lista de juegos. Las pruebas deben superarse antes de combinar los cambios.

1. Continúa la conversación actual o inicia otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.
3. Confirma que está seleccionado **Agent**.
4. Confirma que está seleccionado **Claude Sonnet 4.5**.
5. Envía el prompt:

   ```plaintext
   Actualiza el sitio para incluir el filtrado por editorial y categoría según los requisitos de la incidencia de GitHub correspondiente del backlog. El helper de editoriales ya existe; conserva y perfecciona ese trabajo cuando sea necesario y completa la capa de datos, la interfaz y las pruebas restantes. Asegúrate de que todas las pruebas se superan antes de terminar. El servidor ya está en ejecución; no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto, localiza los helpers, la interfaz y las pruebas, modifica los archivos y ejecuta las pruebas.
7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.
8. Puede que se produzcan pausas o que algunas pruebas fallen durante el proceso. Copilot alternará entre cambios y pruebas hasta que termine y no detecte errores.
9. Revisa el código generado.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas comprobado el resultado y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y pasos siguientes

Has utilizado el modo agente de GitHub Copilot para añadir funcionalidades al sitio de Tailspin Toys, explorar el proyecto, modificar la interfaz y la capa de datos, y revisar cambios y pruebas.

A continuación, [probarás la funcionalidad con Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

Si la lista de juegos sigue creciendo, puede ser necesario añadir paginación. Pide a Copilot que implemente esa funcionalidad con las prácticas recomendadas y los archivos de instrucciones existentes. Decide si el usuario podrá elegir el tamaño de página o si será fijo, y aporta suficiente contexto en el prompt. Es posible que tengas que iterar y aclarar los cambios; es parte normal del trabajo con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: diferencias y usos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
