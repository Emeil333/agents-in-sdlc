---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan deuda técnica: incidencias de seguridad pendientes, código heredado que necesita actualizarse o solicitudes de funcionalidad relegadas en el backlog por falta de tiempo. El agente de GitHub Copilot en la nube puede realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Al terminar, crea una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así, el equipo puede delegar tareas tediosas, acelerar el desarrollo y centrarse en asuntos de mayor alcance.

Con el agente de Copilot en la nube explorarás cómo:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: en realidad, no hay documentación. El primer paso será añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego, para que los usuarios puedan seguir explorando. No necesita una implementación pulida todavía; solo algo que permita probar y aceptar la experiencia de usuario. Esta funcionalidad supone un bloqueo ahora mismo, aunque hay otras incidencias con mayor prioridad.

Es fácil que estas tareas se pospongan, por lo que son buenas candidatas para asignarlas al agente de Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, revisar su trabajo y confirmar que cumple los requisitos.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una tarea, Copilot crea una PR en borrador para hacer un seguimiento, configura el entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o una vez terminada. Cuando la solución esté lista para que la revises, Copilot te mencionará en la PR.

## La importancia de definir bien el alcance

GitHub Copilot no hace magia: no elimina la necesidad de pensar. Incluso las tareas que parecen sencillas pueden ser complejas al analizarlas en detalle. Por eso, [define cuidadosamente el alcance de las tareas para el agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo guían a Copilot. Si ya completaste el ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo se utiliza `.github/copilot-instructions.md`; si no, puedes revisarlo ahora. Antes de asignar trabajo al agente, consulta en modo de solo lectura los archivos de instrucciones del repositorio para observar su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un Codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Falta documentación en el código`, revisa la PR para ver los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario. Estas directrices proceden de los archivos de instrucciones; volveremos a ellas al revisar las PR en un ejercicio posterior.

## Configurar el entorno de desarrollo para el agente de Copilot en la nube

El desarrollo de código suele requerir un entorno específico y scripts de configuración. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma similar a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] para configurar el entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. La plantilla ya lo ha configurado para este laboratorio. Instala Node.js, las dependencias del proyecto y los binarios del navegador para las pruebas de extremo a extremo, y prepara la base de datos SQLite migrada y con datos para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para estos pasos. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si estos pasos clonan el repositorio, por ejemplo, para instalar dependencias, se necesita el permiso contents: read. Si no se clona el repositorio aquí, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (toda la aplicación está implementada con Astro y Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que lo ejecute Copilot.

## Añadir documentación

Aunque todos reconocen la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica suele quedar desatendida, reduce la productividad y dificulta mantener el código o incorporar a nuevos desarrolladores. Copilot puede ayudar a crear documentación, así que esta incidencia es una buena tarea para asignarle. Trabajará en segundo plano y volverás a revisar sus cambios en un ejercicio posterior.

1. En una nueva pestaña del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Falta documentación en el código`.
6. En **Description**, escribe:

   ```plaintext
   Nuestra organización exige que las funciones y los métodos incluyan comentarios TSDoc cuando sean útiles. Por desgracia, los cambios recientes no han seguido este estándar. Hay que actualizar el código existente para añadir comentarios de documentación cuando ayuden a aclarar el comportamiento.
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

   ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva PR, cuyo título será parecido a `[WIP]: Falta documentación en el código`. Si todavía no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno mediante el flujo de trabajo anterior, analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas previstas.
14. Desplázate por la conversación de la PR y confirma que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Explora la sesión en directo para ver cómo Copilot investiga el código, analiza su estado, planifica y crea los cambios.

El trabajo puede tardar varios minutos. Uno de los objetivos del agente en la nube es trabajar de forma asíncrona y permitirte centrarte en otras tareas. Aprovecharás esa capacidad asignándole otra tarea y, mientras tanto, escribirás código para añadir funcionalidades a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente de Copilot en la nube es que permite dividir el trabajo: tú puedes ocuparte de unas tareas mientras el agente trabaja en otras. Añadir una sección de juegos relacionados quizá no lleve mucho tiempo, pero podrías dedicar ese tiempo a otras tareas. Asígnale el trabajo al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Mostrar juegos relacionados en la página de detalles`.
6. En **Description**, escribe:

   ```markdown
   Queremos ayudar a descubrir más juegos mostrando juegos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario y realizar pruebas de aceptación. Estos son los requisitos:

   - Añadir un helper de acceso a datos en `src/lib/` que devuelva otros juegos de la misma categoría que uno determinado, sin incluir ese juego.
   - Mostrar en la página de detalles una sección «Juegos relacionados» que utilice el helper.
   - Gestionar el caso en que un juego no tenga juegos relacionados.
   - Crear pruebas unitarias para el nuevo helper.
   - Asegurarse de que se superan todas las pruebas antes de crear la PR.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el panel de la derecha, selecciona **Assign to Copilot**.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, aparecerá el emoji 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

El agente está trabajando en la segunda tarea. Como trabaja de forma asíncrona, no hace falta supervisarlo continuamente; puedes revisar el resultado cuando termine. Mientras tanto, aprenderás a crear y usar agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents], que permite asignarle incidencias para que trabaje de forma asíncrona. Puedes usarlo para resolver deuda técnica, crear funcionalidades o ayudar a migrar código entre frameworks. Has configurado el entorno, revisado la seguridad, definido el alcance y asignado incidencias.

Mientras el agente trabaja, pasarás a crear y usar agentes personalizados. El [agente en la nube también puede usar servidores MCP][cloud-agent-mcp] y acceder a las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
