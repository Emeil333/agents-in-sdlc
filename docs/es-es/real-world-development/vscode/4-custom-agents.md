---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de IA especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definirlos en archivos Markdown dentro de `.github/agents`, puedes proporcionar instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos para que Copilot realice determinados trabajos con más eficacia. Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices [WCAG][wcag], uno de seguridad o uno de pruebas.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML con, como mínimo, `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común: ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La diferencia más sencilla es que el **agente personalizado** es quien realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente de accesibilidad revisará el sitio según las directrices de accesibilidad. Como parte de su trabajo, podría utilizar habilidades para gestionar solicitudes de incorporación de cambios o ejecutar pruebas.

> [!NOTE]
> No hay una única manera correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba distintas opciones y adáptalas al entorno y a las necesidades.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas. Algunos usuarios han señalado que el tema oscuro actual es difícil de leer porque el contraste entre el texto y el fondo es insuficiente. Para resolverlo, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es importante, utilizarás un agente personalizado para implementar esta funcionalidad cuanto antes.

## Revisar el agente de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

Vuelve a tu Codespace y crea una rama nueva a partir de `main` para mantener separado el trabajo de accesibilidad de la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen las responsabilidades principales, las prácticas de accesibilidad, ejemplos de HTML, CSS y JavaScript, y errores habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados de `.github/agents` en la lista de agentes de Copilot Chat. Puedes seleccionar uno para usar sus instrucciones y herramientas durante una conversación.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de continuar, vuelve a tu Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo dependen del ejercicio; aquí se indican cuando corresponde.

1. Selecciona **Agent** en la lista de agentes si todavía no está seleccionado.
2. Abre la lista de agentes de la parte inferior de la vista Chat, donde aparece el agente activo (de forma predeterminada, **default**).
3. Selecciona **Accessibility agent**.
4. Envía al agente este prompt:

   ```plaintext
   Añade un modo de alto contraste al sitio. Debe haber un conmutador para que el usuario pueda activar el modo y la preferencia debe conservarse entre recargas de la página mediante el almacenamiento local del navegador.
   ```

5. Copilot explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y las diferencias para que puedas revisarla.
6. El indicador **Files changed** se actualizará a medida que el agente modifique archivos.

> [!NOTE]
> Este proceso puede tardar unos minutos. Copilot modificará archivos reales del repositorio, como componentes Astro, CSS y pruebas relacionadas.

En el siguiente ejercicio revisarás y dirigirás el trabajo en curso.

## Resumen y pasos siguientes

Has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados en tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables.

En este ejercicio has aprendido cómo se definen los agentes personalizados y cómo se usan en el modo agente de Copilot Chat. A continuación, [supervisarás y dirigirás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un conmutador de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados de awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
