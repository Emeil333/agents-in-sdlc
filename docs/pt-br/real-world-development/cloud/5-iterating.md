---
title: "Exercício 5: Itere sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Monitoramento e gerenciamento de agentes][previous-lesson] |
|:--|

## Revise o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design testar e implementasse recursos de acessibilidade, incluindo controles de modo de alto contraste e modo claro. Vamos explorar as alterações e, se necessário, fornecer comentários para que o Copilot aprimore o trabalho.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a inclusão da IA generativa. Sempre queremos revisar o código gerado e seguir nosso processo normal de DevOps. Portanto, vamos revisar as propostas do GitHub Copilot para documentar o código, criar o recurso de jogos relacionados e implementar os recursos de acessibilidade antes de habilitar a revisão pela equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, foram definidas restrições de segurança para proteger o ambiente. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions, que cria um ambiente separado e temporário para o trabalho.
- Todos os fluxos de trabalho do GitHub Actions precisam da aprovação de uma pessoa antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revise a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: o PR que adiciona documentação ao código. Você fará essa revisão usando a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, talvez você veja um aviso informando que o Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o acesso do Copilot a recursos externos, inclusive a servidores MCP externos, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título semelhante a **Add missing documentation** ou outro título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o PR conterá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho; pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os comentários TSDoc recém-criados e outras documentações. As alterações exatas variam.

    Ao examinar as alterações, procure comentários TSDoc, convenções TypeScript e cabeçalhos de comentários. Eles vêm dos arquivos de instruções personalizadas que você revisou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila da seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Solicite alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você também pode mencionar o Copilot nos comentários do PR ou em comentários de linha no código, assim como faria com outras pessoas da equipe. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não há um texto prescritivo para o pedido. Algumas sugestões:

- Adicionar cabeçalhos de comentários no início de cada arquivo de código com uma breve descrição da finalidade.
- Adicionar comentários TSDoc a arquivos TypeScript e Astro.
- Criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das sugestões acima ou peça outra melhoria de documentação para a base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, deverá aparecer um novo commit no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, converta o PR de rascunho em pronto para revisão e faça o merge na branch principal.

![Converter o PR em pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Agora, vamos voltar ao PR criado pelo Copilot para resolver a issue de exibição de jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page** ou outro título mais descritivo.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
6. Você deverá ver um indicador de que alguns fluxos de trabalho aguardam aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Os fluxos de trabalho deverão entrar na fila da seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** Para testar manualmente o recurso de jogos relacionados, selecione essa branch no Codespace. Acesse o Codespace, abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode solicitar atualizações ao agente de nuvem de forma iterativa. Por exemplo, peça ao Copilot que ajuste a quantidade de jogos relacionados exibidos ou adicione cabeçalhos de comentários e comentários TSDoc (lembre-se: a issue foi atribuída **antes** da atualização das instruções personalizadas). Para isso, adicione um comentário na guia **Conversation**. O Copilot o verá e iniciará uma nova sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados pelo agente personalizado de acessibilidade. Este PR deve incluir o modo de alto contraste que você solicitou no Exercício 3 e o modo claro solicitado no centro de controle no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website** ou outro título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request conterá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho; pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação e preste atenção especial a:
   - Os componentes de interface para alternar entre os modos.
   - O uso do armazenamento local para manter as preferências do usuário.
   - As alterações de CSS ou estilo dos modos de alto contraste e claro.
   - Os atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.).
   - O código JavaScript/TypeScript que controla a alternância entre os modos.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila da seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** Você pode selecionar essa branch no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os controles de modo de alto contraste e modo claro no navegador. Confirme se funcionam e se as preferências continuam salvas depois de recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos seguindo boas práticas de acessibilidade. Se encontrar problemas ou melhorias de acessibilidade, mencione **@copilot** em um comentário e solicite uma atualização, como fez com os PRs anteriores.

## Exercício opcional — continue delegando

O agente de nuvem funciona melhor quando você pode atribuir itens reais da lista de pendências e se concentrar em outras tarefas. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- Criar um formulário para manifestar interesse em apoiar o projeto na página de detalhes do jogo.
- Implementar paginação na lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o roteiro do agente de nuvem. Nestas lições, você:

- **Inspecionou os arquivos de instruções personalizadas incluídos no repositório** para ver como influenciariam mais tarde o resultado do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** de acessibilidade para adicionar controles de modo de alto contraste e modo claro.
- **Usou a página de agentes do Copilot como centro de controle** para monitorar e orientar a sessão de acessibilidade em andamento.
- **Revisou e ajustou os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximas etapas

Você concluiu o roteiro do agente de nuvem. Para continuar explorando, os outros roteiros complementam o que você praticou:

- 🖥️ **[Roteiro do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Roteiro da CLI](../../cli/)** — entregue duas alterações revisadas no terminal usando os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, Playwright MCP, Agent Merge e uma ponte opcional `/delegate` para voltar ao agente de nuvem.

No seu repositório, experimente estas próximas etapas:

- Atribua ao agente de nuvem uma issue de refatoração ou de cobertura de testes.
- Crie um agente personalizado para outro domínio.
- Configure `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Monitoramento e gerenciamento de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
