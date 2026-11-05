---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información adicional, asegúrate de que tenga acceso a ella. Los [archivos de instrucciones][instruction-files] proporcionan indicaciones sobre cómo generar código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones del repositorio y los archivos aplicables a determinadas rutas.
- generarás la primera parte de la capa de datos para el filtrado (un helper de editoriales) con las instrucciones actuales.
- añadirás un estándar general a `.github/copilot-instructions.md`.
- repetirás el mismo prompt y observarás cómo el código generado incorpora el nuevo estándar.
- confirmarás los cambios de instrucciones y del filtrado en `main` para que Copilot pueda utilizar las nuevas directrices en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no seguir todos los estándares establecidos. Copilot no es determinista. El objetivo es observar la tendencia del cambio después de actualizar las instrucciones, no obtener una salida idéntica carácter por carácter.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un diseño moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones te asegurarás de que Copilot disponga de la información adecuada para seguir estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Puedes indicar las convenciones, las bibliotecas y los tipos de comentarios que quieres utilizar. Las instrucciones pueden aplicarse a todo el repositorio o a determinados tipos de archivo.

Hay dos tipos de archivos:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información pertinente para la mayoría de las solicitudes, como la pila tecnológica, una descripción del proyecto y las prácticas recomendadas.
- `.github/instructions/*.instructions.md`, archivos que ofrecen directrices para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> En el IDE, las instrucciones se utilizan para generar código en Copilot Chat, pero no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` (con frontmatter `applyTo`) al generar código.
>
> Copilot también [admite otros formatos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar las instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en las directrices generales del proyecto, como su descripción, estructura y estándares de programación.
- Usa archivos `*.instructions.md` para tipos de archivo (pruebas unitarias, componentes Astro o capa de datos) y tareas concretas.
- Escribe en lenguaje natural, con directrices claras y ejemplos de código correctos e incorrectos.

No hay una única manera de crear archivos de instrucciones ni de usar la IA. La experimentación te ayudará a encontrar el enfoque más adecuado.

> [!TIP]
> Los proyectos que usan GitHub Copilot deberían tener una colección sólida de archivos de instrucciones. En este proyecto encontrarás archivos para tareas como [actualizar la interfaz][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada interfaz lo permite de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI.
>
> Para encontrar plantillas y ejemplos, explora [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas

Lee los archivos incluidos en el repositorio: `.github/copilot-instructions.md` y varios archivos `*.instructions.md`.

1. Abre `.github/copilot-instructions.md`.
2. Explora la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas **GitHub Actions Workflows**.
3. Explora `.github/instructions`, donde hay instrucciones para Astro, Drizzle, pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en `applyTo`, que define el glob de los archivos afectados; por ejemplo, `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus enlaces a otros archivos de instrucciones y a ejemplos del proyecto. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> **Code formatting requirements** documenta los estándares de programación, pero todavía no exige documentación dentro del código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de archivo.

## Crear una rama para los cambios

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Vuelve al Codespace, abre Copilot Chat y selecciona **New Chat**. El modo y el modelo dependen del ejercicio; se indican en cada paso.

Primero enviarás un prompt con las instrucciones actuales para observar su efecto. Después actualizarás las instrucciones y repetirás el mismo prompt.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo disponga del contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar dónde debe estar el helper.
3. Selecciona **Agent** en la lista de agentes de Chat.
4. Envía el prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todas las editoriales con su nombre e id. Aplica los cambios al archivo.
   ```

5. Copilot explorará el proyecto y aplicará cambios, posiblemente en el helper y sus pruebas.
6. Observa que el helper es una función tipada que recibe un cliente `db` como primer argumento y devuelve un array tipado de editoriales. Sigue las convenciones de `.github/instructions/drizzle.instructions.md`, que se aplican a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** comentarios TSDoc y un encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, podría añadir comentarios de documentación aunque no se lo pidas. No pasa nada: el objetivo es observar cómo mejora la coherencia tras actualizar las instrucciones.

## Añadir un estándar general

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca **Code formatting requirements**, cerca de la línea 27. Todavía no incluye una regla de documentación dentro del código.
3. Añade estas líneas de Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones generales del repositorio van en `.github/copilot-instructions.md`; las específicas para un lenguaje, tipo de archivo o tarea pueden ir en archivos `*.instructions.md`.

## Repetir el prompt y observar el cambio

1. En Copilot Chat, selecciona **New Chat** para borrar el contexto anterior.
2. Vuelve a seleccionar `src/lib/publishers.ts`.
3. Envía el mismo prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todas las editoriales con su nombre e id. Aplica los cambios al archivo.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios similar a este:

   ```typescript
   /**
    * Helpers de acceso a datos de editoriales para la plataforma de Tailspin Toys.
    * Proporciona funciones para recuperar datos de editoriales de la base de datos.
    */
   ```

5. Observa que la función incluye un comentario TSDoc parecido al siguiente:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su id y nombre.
    *
    * @param db - El cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en un array de objetos de editoriales.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique al código que utilizarás en el ejercicio siguiente.

## Confirmar las instrucciones y enviar la rama

1. Abre un terminal y comprueba los cambios:

   ```bash
   git status
   ```

2. Prepara y confirma las instrucciones y todos los archivos del helper que haya modificado Copilot:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados, como pruebas, añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

3. Envía la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de incorporación de cambios

Con la rama publicada, crea una solicitud de incorporación de cambios (PR) relacionada con la incidencia sobre los estándares de documentación del backlog.

> [!NOTE]
> En este ejercicio Copilot actuará por primera vez en GitHub. La plantilla del proyecto ya configura las herramientas de GitHub. Puede que VS Code te pida iniciar sesión la primera vez que Copilot use una herramienta; sigue las indicaciones. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir Chat y confirma que está seleccionado **Agent**.
3. Pide a Copilot que encuentre la incidencia y cree una PR desde la rama actual:

   ```plaintext
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una PR desde la rama actual, indica que cierra esa incidencia e incluye que también hemos añadido la base del helper de editoriales para el próximo trabajo de filtrado.
   ```

4. Revisa cada acción de GitHub que Copilot solicite y selecciona **Allow** cuando corresponda.
5. Cuando se cree la PR, pide a Copilot que la combine en `main` y actualice la rama local:

   ```plaintext
   Combina la PR en main. Después, vuelve a main en local y descarga el código más reciente.
   ```

6. Revisa y autoriza las acciones de GitHub y los comandos de shell que solicite Copilot.

¡Has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y pasos siguientes

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones y has usado Copilot Chat en VS Code para generar código con las instrucciones existentes, añadir un estándar general, repetir el prompt y confirmar los cambios en `main`.

A continuación, aplicarás las instrucciones en [modo agente][next-lesson] mientras Copilot añade una funcionalidad al código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales de GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
