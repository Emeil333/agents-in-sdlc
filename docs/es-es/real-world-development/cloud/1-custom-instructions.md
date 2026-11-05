---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita conocer información de fondo, asegúrate de que tenga acceso a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo *qué* debe hacer, sino también *cómo*.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos para determinadas rutas.
- añadirás un nuevo estándar general del repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no ejecutarás un prompt antes y otro después para comparar resultados. El agente de Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás cómo influyen las instrucciones más adelante, al revisar las PR que genere el agente.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo, entre ellos:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un diseño moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones te asegurarás de que Copilot conozca estas prácticas y pueda seguirlas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que entienda mejor tu estilo de programación y tus requisitos. Puedes especificar las convenciones de programación, las bibliotecas y los tipos de comentarios que quieres utilizar. Las instrucciones pueden aplicarse a todo el repositorio o a determinados tipos de archivo.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto que sea pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción de lo que se está creando, las prácticas recomendadas y otras directrices generales.
- `.github/instructions/*.instructions.md`, archivos que ofrecen directrices para tareas o tipos de archivo concretos. Por ejemplo, pueden contener pautas para lenguajes como TypeScript o Astro, o para crear componentes de interfaz o conjuntos de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones se utilizan para generar código en Copilot Chat, pero no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` (con frontmatter `applyTo`) al generar código.
>
> Copilot también [admite otros formatos de archivos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

En el proyecto de ejemplo encontrarás un enfoque representativo para crear archivos de instrucciones. En términos generales:

- Mantén las instrucciones de `copilot-instructions.md` centradas en directrices del proyecto: una descripción de lo que se está creando, la estructura del proyecto y los estándares globales de programación.
- Usa archivos `*.instructions.md` para instrucciones específicas de tipos de archivo, como pruebas unitarias, componentes Astro y la capa de datos, o para tareas concretas.
- Usa lenguaje natural y directrices claras. Incluye ejemplos de cómo debería ser el código y de cómo no debería ser.

No hay una única manera de crear archivos de instrucciones, igual que no hay una única manera de usar la IA. La experimentación te ayudará a encontrar el enfoque más adecuado para el proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener una colección sólida de archivos de instrucciones. En este proyecto encontrarás archivos para muchos tipos de tareas, entre ellas [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada interfaz lo permite de una manera distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). La lección correspondiente lo explicará cuando sea pertinente.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Encontrarás instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo`: define un glob relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. Por ejemplo, coincidirá con cualquier archivo de pruebas TypeScript que se ajuste a `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y ve al final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que debe seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentación dentro del código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentario de los archivos.

## Añadir un nuevo estándar del repositorio

Este es el único cambio que harás en este ejercicio: añadir una regla general para que la documentación se incluya en el código mediante comentarios TSDoc y un encabezado de comentario a nivel de archivo. El agente en la nube aplicará esta regla cuando trabaje en incidencias que le asignes más adelante.

Antes de editar el archivo, prepara una rama. Aunque el agente leerá las instrucciones de la rama de destino de la incidencia, confirmarás los cambios de forma ordenada:

1. En el terminal del Codespace, crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Añade los estándares de programación que ayuden a generar mejores sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que todavía no contiene directrices para la documentación dentro del código; por eso el helper generado no incluía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo después de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, las instrucciones pueden aplicarse a todo el repositorio mediante `.github/copilot-instructions.md` o a lenguajes, tipos de archivo y tareas concretos mediante archivos `*.instructions.md`. El archivo general del repositorio es el lugar adecuado para estándares como la regla de comentarios de documentación que acabas de añadir.

## Confirmar, enviar y combinar la actualización

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio, Copilot creará una rama a partir de `main`; por eso, los cambios de instrucciones deben estar en `main` para que el agente los utilice.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así el agente podrá leer estas instrucciones en el siguiente ejercicio.

> [!TIP]
> Si para este taller prefieres trabajar directamente en `main`, puedes omitir la rama y confirmar los cambios directamente en `main`. El paso de la rama permite practicar un flujo de trabajo habitual en un proyecto real.

## Resumen y pasos siguientes

Has explorado cómo utiliza Copilot los archivos de instrucciones de este proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Durante el resto del itinerario comprobarás el efecto de ese estándar en las PR que genere el agente en la nube.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
