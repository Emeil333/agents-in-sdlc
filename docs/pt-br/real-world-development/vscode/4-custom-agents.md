---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você pode fornecer instruções focadas, boas práticas, padrões de programação e conhecimento específico do domínio para que o Copilot execute determinados tipos de trabalho com mais eficiência. As equipes podem transformar sua experiência em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica a conformidade com as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou, globalmente, em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido por um prompt em Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como executar tarefas. A distinção mais simples é: o **agente personalizado** é quem executa o trabalho, e as **skills** são as ferramentas.

Agentes personalizados têm sua própria janela de contexto e coordenam skills e até outros agentes para executar o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade; nesse trabalho, ele poderá chamar skills, como uma skill para o fluxo de pull requests ou para executar e gerenciar testes.

> [!NOTE]
> Não existe uma única forma certa de criar um agente personalizado. Como acontece com qualquer recurso de IA, teste e itere para encontrar o que funciona melhor nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre as cores do texto e do fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é essencial, queremos implementar esse recurso o mais rápido possível. Para isso, usaremos um agente personalizado.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado. Vamos revisar o arquivo para entender como ele orientará o Copilot.

Volte ao Codespace e abra um terminal. Selecione uma branch nova a partir de `main` para o trabalho de acessibilidade (o PR de filtragem do Exercício 3 ficará separado):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Leia as seções seguintes, que destacam:
   - As principais responsabilidades ao gerar código para um site acessível.
   - Boas práticas de acessibilidade.
   - Exemplos de código HTML, CSS e JavaScript.
   - Erros e problemas comuns.

## Use o agente personalizado no Copilot Chat

O VS Code exibe todos os agentes personalizados definidos em `.github/agents` no menu suspenso de agentes, na parte inferior da exibição Copilot Chat. Você pode selecionar um agente personalizado para aplicar as instruções e ferramentas dele à sessão de chat.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; as etapas indicarão quando for relevante.

1. No menu suspenso de agentes da exibição Chat, selecione **Agent**, caso ainda não esteja selecionado.

   ![Seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione o menu suspenso de agentes na parte inferior da exibição Chat. Ele mostra o agente ativo, que por padrão é **default**.
3. Na lista de agentes disponíveis, selecione **Accessibility agent**.
4. Envie o prompt a seguir ao agente de acessibilidade:

    ```plaintext
    Adicione um modo de alto contraste ao site. Deve haver um controle que permita ativá-lo, e a configuração deve continuar salva no armazenamento local do navegador após recarregar a página.
    ```

5. O Copilot Chat explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para você revisar.
6. À medida que as edições forem feitas, o indicador **Files changed** será atualizado para mostrar o conjunto de arquivos alterados pelo agente.

> [!NOTE]
> Esse processo provavelmente levará alguns minutos. O Copilot está fazendo alterações reais no repositório: editará arquivos existentes, como componentes Astro e CSS, e talvez também testes relacionados.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximas etapas

Neste exercício, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Com eles, é possível transformar o conhecimento e os padrões da equipe em agentes reutilizáveis que orientam o Copilot a executar determinados tipos de trabalho com mais eficiência.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Na próxima lição, você vai [monitorar e orientar o trabalho do agente][next-lesson], revisar as alterações em andamento e adicionar um controle do modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Teste o recurso com o servidor Playwright MCP][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
