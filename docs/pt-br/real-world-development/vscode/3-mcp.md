---
title: "Exercício 3 - Teste o recurso com o servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request, confirme se ele realmente funciona no navegador. Em vez de navegar pelo aplicativo manualmente, conecte o **servidor Playwright MCP** e deixe o Copilot controlar um navegador real para testar o recurso. Em seguida, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionar o servidor Playwright MCP ao workspace.
- pedir ao Copilot que use o servidor para testar manualmente o recurso de filtragem em um navegador.
- publicar sua branch e abrir um pull request com as alterações de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente se torna muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) permite isso: é um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama mostrando o funcionamento interno do modo de agente e como ele interage com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes podem acessar informações atualizadas (usando recursos) e executar ações em seu nome (usando ferramentas).

Essas ferramentas e esses recursos são acessados por um servidor MCP, que funciona como ponte entre o agente de IA e as ferramentas e serviços externos. O servidor MCP gerencia a comunicação entre o agente e as ferramentas externas, como APIs existentes ou ferramentas locais, por exemplo, pacotes npm. Cada servidor MCP representa um conjunto de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP conhecidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: dá acesso a APIs para gerenciar repositórios do GitHub, permitindo que o agente crie repositórios, atualize os existentes e gerencie issues e pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação de navegadores com o Playwright, permitindo que o agente acesse páginas, preencha formulários e selecione botões.

Há muitos outros servidores MCP que dão acesso a ferramentas e recursos diferentes. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta e a contribuição para esse ecossistema.

> [!CAUTION]
> Por segurança, trate os servidores MCP como qualquer outra dependência do seu projeto. Antes de usar um servidor MCP, revise cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores MCP confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` vem do template do projeto. Por isso, o Copilot conseguiu ler a lista de pendências no exercício anterior. Ela usa o [servidor remoto GitHub MCP][remote-github-mcp-server], então não é preciso instalar nada localmente: o VS Code se conecta por HTTP e você se autentica no GitHub na primeira vez que o Copilot usa uma das ferramentas.

## Adicione o servidor Playwright MCP

Agora você adicionará um segundo servidor. O [servidor Playwright MCP][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, exatamente o que você precisa para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia no computador executando `npx @playwright/mcp@latest`. A opção `--headless` faz o Playwright executar o navegador sem uma janela visível. Isso é necessário em um Codespace, que não tem uma área de trabalho para exibir a janela.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright nos testes de ponta a ponta, então o navegador necessário costuma estar instalado. Se o Copilot informar que não há um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia os servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pede que você confirme se confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor e confirmar se a configuração está correta.
2. Se o VS Code pedir que você confirme se confia no servidor, revise a configuração e escolha a opção de confiança para iniciá-lo.

Iniciar o servidor manualmente é opcional. Se você pular essa etapa, o VS Code iniciará o servidor e pedirá sua confirmação na primeira vez que o Copilot precisar das ferramentas, na próxima etapa.

## Teste o recurso de filtragem

O [servidor Playwright MCP][playwright-mcp-server] fornece ao Copilot um navegador real para controlar. Em vez de navegar pelo aplicativo manualmente, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumi-los para você. É uma maneira rápida de confirmar se o recurso funciona como esperado, sem sair da conversa.

Nos bastidores, o servidor Playwright MCP usa a [árvore de acessibilidade][playwright-mcp-server] da página em vez de capturas de tela. Assim, o agente raciocina sobre elementos estruturados e rotulados (botões, links e itens de lista), como a tecnologia assistiva faz. Dessa forma, uma verificação funcional rápida também permite conferir alguns aspectos básicos de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso de filtragem criado:

```text
Usando o servidor Playwright MCP, abra um navegador no aplicativo em execução em http://localhost:4321 e verifique o novo recurso de filtragem de jogos:

1. Acesse a página de jogos e anote quantos jogos estão listados.
2. Aplique um filtro de categoria e confirme se a lista passa a exibir somente jogos dessa categoria.
3. Limpe o filtro e aplique um filtro de editora. Confirme se a lista passa a exibir apenas jogos dessa editora.
4. Combine os filtros de categoria e editora e confirme se os resultados correspondem a ambos.

Relate o que observou em cada etapa e destaque qualquer comportamento inesperado.
```

O Copilot abrirá um navegador pelo servidor Playwright MCP, seguirá cada etapa e apresentará o resultado. Compare o resumo com os critérios de aceitação da issue. Se algo não parecer correto, faça perguntas ou peça que o Copilot corrija o código antes de abrir o pull request.

> [!NOTE]
> Para este teste, o aplicativo precisa estar em execução em `http://localhost:4321`. Se você tiver interrompido o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor Playwright MCP, talvez seja necessário baixar um navegador. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a branch e crie um pull request

Agora que confirmou que o recurso funciona, você pode abrir um pull request (PR) para que a equipe o revise. Primeiro, publique a branch `filtering-vscode`.

1. No Codespace, acesse o painel **Source Control** e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Painel Source Control mostrando as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie o pull request

Há várias maneiras de criar um pull request, inclusive pelo github.com e pela interface de linha de comando do GitHub (CLI). Como você já está usando o GitHub Copilot, vamos pedir a ele que crie o PR. Ele pode localizar a issue correspondente e associá-la ao PR.

1. Acesse o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme se **Agent** está selecionado no menu suspenso de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Encontre no repositório a issue relacionada à filtragem por categoria e editora. Crie um pull request para a branch atual e associe-o à issue correta.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot obtenha as informações e execute as operações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja necessário autenticar-se. Siga as instruções para autorizar.
5. Observe o Copilot pesquisar as issues, encontrar a correta e criar o PR.
6. Selecione o link exibido pelo Copilot para revisar o pull request, mas **não faça o merge ainda**.

## Resumo e próximas etapas

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionou o servidor Playwright MCP ao workspace.
- usou o servidor para testar manualmente o recurso de filtragem em um navegador antes da publicação.
- publicou sua branch e abriu um pull request com as alterações de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma das maneiras de ampliar as capacidades do Copilot. Na próxima lição, vamos [criar um agente personalizado][next-lesson] para agilizar tarefas focadas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todos estão falando sobre isso?][mcp-blog-post]
- [Servidor Playwright MCP da Microsoft][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
