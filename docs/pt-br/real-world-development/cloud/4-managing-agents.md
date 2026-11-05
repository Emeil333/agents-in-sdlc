---
title: "Exercício 4 - Monitoramento e gerenciamento de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas diferentes para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma assíncrona e autônoma, ainda é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse centro de controle, você vê todas as tarefas dos agentes que têm pull requests (PRs) abertos. Também pode explorar as operações realizadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para monitorar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar mais funcionalidades.

## Cenário

Depois que a equipe atribuiu ao agente a criação do modo de alto contraste, percebeu que seria uma boa oportunidade para adicionar também um modo claro. Como o agente já estava atualizando o estilo do site e adicionando controles de alternância, pareceu lógico incluir esse recurso. Você orientará o agente para garantir que ele implemente tanto o modo claro quanto o alto contraste.

## Revise as tarefas do agente de nuvem do Copilot

Vamos conferir o status atual de todas as tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
    - Atualizar a documentação da base de código.
    - Gerar APIs para modificar jogos.
    - Adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento. Revise as operações já executadas pelo Copilot, que podem incluir:
    - Clonar o código do repositório.
    - Criar o ambiente de trabalho do Copilot.
    - Configurar servidores MCP.
    - Executar as várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exatas exibidas variam conforme o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull requests à direita. Ele permite conferir o PR e os arquivos alterados para acompanhar o trabalho.

## Oriente o agente de nuvem

Agora que você viu as tarefas em andamento, vamos pedir ao Copilot que inclua um controle do modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente à adição do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no centro de controle](../../../_images/ex6-accessibility-session.png)

2. Observe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Isso ficará evidente quando surgirem mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o prompt:

    ```plaintext
    Enquanto trabalhamos no modo de alto contraste, vamos adicionar também um modo claro. Deve haver um controle para esse modo, permitindo que as pessoas selecionem a aparência desejada.
    ```

    ![Tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para orientar a sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

Assim como antes, isso levará alguns minutos. Aproveite para fazer uma pausa e refletir sobre o que você aprendeu e explorou até aqui.

## Resumo e próximas etapas

Neste exercício, você explorou a página de agentes do Copilot, o centro de controle para monitorar e orientar tarefas do agente de nuvem do GitHub Copilot. Nela, você acompanha tarefas ativas e concluídas, revisa o trabalho em andamento e redireciona sessões para ajustar o escopo ou fornecer orientações adicionais.

Você explorou estes conceitos:

- usar o centro de controle e a página de agentes para monitorar tarefas do agente de nuvem.
- redirecionar uma sessão em andamento para solicitar mais funcionalidades.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
