---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não enfrentem problemas de dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficaram paradas na lista de pendências por falta de tempo. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando conclui o trabalho, ele cria um rascunho de PR para revisão de uma pessoa desenvolvedora. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em objetivos mais amplos.

Neste exercício, você vai explorar como o agente de nuvem do Copilot pode:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- trabalhar com issues de escopo bem definido.
- receber issues atribuídas.

## Cenários

A Tailspin Toys tem dívidas técnicas que gostaria de resolver. As pessoas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado: na prática, ela está totalmente ausente. Como primeira etapa, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Cada página de detalhes de um jogo deve exibir jogos relacionados — outros títulos da mesma categoria — para que as pessoas possam continuar navegando. Ainda não é necessário um resultado refinado; a equipe quer algo que possa usar em testes de aceitação da experiência. O trabalho é atualmente um bloqueio, mas outras issues têm prioridade maior no momento.

Esses dois exemplos de tarefas podem ser facilmente adiados, por isso são bons candidatos para atribuição ao agente de nuvem do Copilot. O agente trabalha nelas de forma assíncrona, enquanto você se concentra em outras tarefas e depois volta para revisar o trabalho e confirmar se está correto.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de maneira semelhante a uma pessoa desenvolvedora. Assim como no trabalho com uma pessoa da equipe, há várias maneiras de atribuir tarefas, inclusive [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria um rascunho de pull request para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão enquanto o trabalho está em andamento ou depois de concluído. Quando a solução proposta estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções com escopo bem definido

O GitHub Copilot não faz mágica: você ainda precisa pensar. Mesmo operações aparentemente simples podem se tornar complexas quando examinadas em detalhes. Por isso, [defina com cuidado o escopo das tarefas do agente de nuvem][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte a abordagem. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já concluiu o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para dar uma olhada. Antes de atribuir trabalho ao agente de nuvem, examine somente para leitura os arquivos de instruções já incluídos no repositório, para que depois você possa reconhecer o efeito deles.

Abra os arquivos a seguir na interface web do GitHub ou em um Codespace, caso já tenha um em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, principalmente as expectativas sobre convenções TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Quando atribuir ao agente de nuvem a issue `Code lacks documentation`, observe no pull request resultante os comentários TSDoc, as convenções TypeScript e os cabeçalhos de comentários. Esses elementos vêm dos arquivos de instruções. Você os verá novamente ao revisar os PRs em um exercício posterior.

## Prepare o ambiente de desenvolvimento do agente de nuvem do Copilot

Desenvolver código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto, independentemente de quem esteja fazendo o trabalho. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo criando um [fluxo de trabalho de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, que será executado antes do início do trabalho. Assim, o agente terá as ferramentas e dependências necessárias. Esse fluxo já foi configurado para facilitar o laboratório e permitir que você aprenda com a experiência. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos navegadores para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no menor nível necessário para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não fizer isso, o Copilot clonará o repositório automaticamente depois que as etapas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (agora, todo o aplicativo usa Astro + Drizzle/libSQL)
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

      # Migra e preenche o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro fluxo de trabalho do GitHub, mas tem alguns pontos importantes:

- Ele contém um único trabalho chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para confirmar que o fluxo funciona sem precisar esperar o Copilot executá-lo.

## Adicione a documentação

Embora todo mundo reconheça a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica costuma passar sem solução, reduzindo a produtividade e dificultando a manutenção da base de código e a integração de novas pessoas à equipe. Felizmente, o Copilot é muito útil para criar documentação, por isso esta é uma issue ideal para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina o **Description** como:

    ```plaintext
    Nossa organização exige que funções e métodos tenham comentários TSDoc quando eles forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários de documentação que esclareçam o comportamento.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer na lista, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente (executando o fluxo de trabalho mencionado anteriormente), analisando o projeto e definindo a melhor abordagem.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do pull request para baixo. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema: explora o código e seu estado, pausa para pensar e definir uma abordagem e, em seguida, cria o código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona, permitindo que você se concentre em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, voltando nossa atenção à criação de funcionalidades para o aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Como já vimos, uma das grandes vantagens do agente de nuvem do GitHub Copilot é dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele trabalha em outro. Talvez a criação de uma seção de jogos relacionados para a equipe de design não leve muito tempo, mas esse tempo também poderia ser dedicado a outras tarefas. Vamos atribuir a tarefa ao agente de nuvem do Copilot.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o template em branco.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina o **Description** como:

    ```markdown
    Queremos ajudar as pessoas a descobrir mais jogos exibindo jogos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência do usuário e fazer alguns testes de aceitação. Estes são os requisitos:

   - Adicionar um helper de acesso a dados em `src/lib/` que retorne outros jogos da mesma categoria que um jogo específico, excluindo o próprio jogo.
   - Exibir uma seção "Related games" na página de detalhes do jogo usando o helper.
   - Tratar o caso em que um jogo não tem jogos relacionados.
   - Criar testes unitários para o novo helper.
   - Confirmar que todos os testes passam antes de criar o PR.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver o emoji 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. Como o agente de nuvem trabalha de maneira semelhante a uma pessoa engenheira de software, não é necessário monitorá-lo continuamente; basta revisar o resultado quando estiver pronto. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximas etapas

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para execução assíncrona e usá-lo para resolver dívidas técnicas, criar recursos ou ajudar a migrar código entre frameworks.

Você explorou estes conceitos:

- personalização do ambiente de geração de código.
- execução segura de operações.
- importância de definir o escopo das issues com clareza.
- atribuição de issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos nos concentrar na criação e no uso de agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso a instruções personalizadas, que foram exploradas em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
