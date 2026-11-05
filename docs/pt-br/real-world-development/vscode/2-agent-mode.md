---
title: "Exercício 2 - Adicione funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma atualização simples de um aplicativo costuma exigir alterações em vários arquivos e a execução de operações, como testes. O fluxo de trabalho de uma pessoa desenvolvedora normalmente envolve localizar todos os arquivos necessários, fazer as alterações, executar testes, depurar, descobrir qual arquivo ficou faltando, fazer outra alteração... e assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para atuar com mais autonomia no IDE. Ele funciona de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos ver como usar o modo de agente para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, com ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona como uma pessoa da equipe e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- revisar as alterações e os testes gerados pelo modo de agente antes de fazer o merge na base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem os jogos por categoria e editora. No exercício anterior, você adicionou um helper de editoras. Agora, concluirá o trabalho restante na camada de dados, na interface e nos testes usando o modo de agente do Copilot.

## Execute o site Tailspin Toys

Antes de fazer alterações, vamos explorar o site Tailspin Toys para entender o que ele faz atualmente.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com um tema voltado a desenvolvedores. Ele permite listar jogos e exibir detalhes de cada um. O site é um único aplicativo Astro, que gera páginas HTML estáticas durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o Drizzle ORM. Não há uma API de back-end separada nem um framework de interface do lado do cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com pequenos elementos `<script>` do Astro, com escopo limitado e APIs padrão do DOM.

### Inicie o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Para executá-la no GitHub Codespace:

1. Volte ao Codespace. Após o exercício anterior, você deve estar na branch `main`.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma nova branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o seguinte comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá uma mensagem com a URL, semelhante a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo pedir para abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um Codespace, selecionar o link `localhost` no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o Codespace, que agora hospeda o servidor web.

### Explore o site

Com o site em execução, explore suas funcionalidades. Os principais recursos incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá mais informações, incluindo título, descrição, editora e categoria.

## Explore a lista de pendências com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; as etapas indicarão quando for relevante.

A primeira versão do site funciona, mas queremos adicionar recursos. Vamos começar revisando a lista de pendências. Ao criar o projeto a partir do template, uma lista de issues foi criada automaticamente. Peça ao GitHub Copilot que a mostre.

1. No menu suspenso de agentes da exibição Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos do terminal e chama ferramentas.

   ![Seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo específico. Durante a criação do workshop, usamos o Claude Sonnet 4.5 e o incluímos nas instruções para que as sugestões de código sejam relativamente consistentes e a experiência seja boa. No entanto, como os LLMs são probabilísticos, as sugestões podem ser diferentes das mostradas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deverá concluir a tarefa.

3. Peça ao Copilot que mostre a lista de pendências enviando o seguinte prompt:

   ```plaintext
   Mostre a lista de pendências do meu repositório do GitHub. Ajude-me a priorizar os itens que serão mais úteis para os usuários.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe como o Copilot também priorizou os itens de acordo com a utilidade que, na avaliação dele, terão para os usuários.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere o código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de instruções da [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que descreve como adicionar funcionalidades ao site.

1. No Codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - Uma visão geral da arquitetura.
   - Princípios de design de componentes, testabilidade e acessibilidade.
   - Links para instruções específicas de tipos de arquivo, incluindo:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Assim, é possível reutilizar instruções, dividir conjuntos complexos em partes menores e usar exemplos e templates.

## Implemente a filtragem

Para concluir a filtragem, será necessário fazer pelo menos três alterações separadas no aplicativo:

- Adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`).
- Adicionar ou atualizar testes do comportamento de filtragem.
- Atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes do merge. O modo de agente do Copilot pode executar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e o arquivo de instruções **ui**.

   ![Exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme se **Agent** continua selecionado no menu suspenso de agentes da exibição Chat.

   ![Seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade conforme os requisitos da issue relacionada na lista de pendências:

   ```plaintext
   Atualize o site para incluir a filtragem por editora e categoria de acordo com os requisitos da issue relacionada na lista de pendências do GitHub. Já existe um helper de editoras do exercício anterior; preserve e aprimore esse trabalho conforme necessário ao concluir as alterações restantes na camada de dados, na interface e nos testes. Confirme se todos os testes passam antes de concluir. O servidor já está em execução, então não é necessário iniciá-lo.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relevantes. Ele deverá encontrar os helpers da camada de dados, a interface e os testes, depois alterar os arquivos e executar os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, alterar arquivos e rodar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, talvez apareçam os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, você poderá aceitar ou desfazer todas as alterações. Não é necessário selecionar esses botões enquanto o trabalho ainda estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez ocorram pausas e alguns testes falhem durante o processo. Não tem problema: o Copilot alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Sessão completa do Copilot Chat no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> É sempre importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador em que o site está em execução e explore o novo recurso.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximas etapas

Parabéns! Neste exercício, vimos como usar o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu como:

- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar as alterações e os testes gerados pelo modo de agente antes de fazer o merge na base de código.

Agora, vamos [testar o recurso com o servidor Playwright MCP][next-lesson] e abrir um pull request.

### Exercício bônus de exploração — implemente a paginação

À medida que a lista de jogos crescer, será necessário habilitar a paginação. Use as habilidades deste exercício e peça ao Copilot que implemente a paginação no site. Considere:

- seguir as boas práticas existentes, incluindo o uso dos arquivos de instruções disponíveis.
- decidir como implementar a paginação: permitir que o usuário escolha o tamanho da página ou definir um valor fixo.
- incluir no prompt as orientações necessárias para que o Copilot implemente o comportamento desejado.
- iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Copilot nos modos ask, edit e agent: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
