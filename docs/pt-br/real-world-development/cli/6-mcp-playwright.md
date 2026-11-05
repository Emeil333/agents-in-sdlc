---
title: "Lição 6 - Valide a funcionalidade com o Playwright MCP"
description: "Inspecione ou configure o Playwright MCP no Copilot CLI e use-o para explorar o recurso de filtragem no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código é apenas parte do trabalho. Também precisamos trabalhar com dados, serviços externos e automações adicionais disponíveis para o Copilot. É aí que entram os servidores MCP. Eles permitem que o Copilot vá além dos recursos integrados à CLI, oferecendo outras ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionar o servidor Playwright MCP, caso ele ainda não esteja disponível.
- pedir ao agente que controle um navegador e explore o recurso de filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar alterações na interface exige interagir com ela. Você quer permitir que o Copilot use o site como uma pessoa usuária faria e automatize ainda mais a validação, aumentando a confiança de que as alterações funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com o MCP, eles podem acessar informações atualizadas e executar ações em seu nome.

As ferramentas e os recursos são acessados por um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente pode acessar.

Alguns servidores MCP conhecidos:

- **[GitHub MCP Server][github-mcp]**: dá acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: oferece recursos de automação de navegadores com o Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta e a contribuição para esse ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do seu projeto. Antes de usar um, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicione o servidor Playwright MCP

Se o servidor Playwright MCP ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária faria.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, abra a caixa de diálogo para adicionar um servidor MCP digitando:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o seguinte na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo MCP.

## Peça ao Copilot para explorar o recurso com o Playwright

Anteriormente, você confirmou manualmente que o recurso funciona. Agora, vamos usar o servidor Playwright MCP disponível no Copilot para fazer o mesmo.

1. Use o prompt a seguir para pedir ao Copilot que valide o recurso com o servidor Playwright MCP:

   ```plaintext
   Inicie o aplicativo e use o Playwright MCP para verificar a filtragem com base na issue e no nosso plano. Informe o que funciona e o que não funciona, sem fazer alterações. Quando terminar, interrompa o servidor iniciado.
   ```

> [!NOTE]
> Você não precisa dizer ao Copilot para usar o servidor MCP, pois ele geralmente o identifica por conta própria. Mas, como você sabe qual ferramenta deve ser usada, indicar isso ajuda a obter resultados mais consistentes e economiza alguns tokens.

2. Observe as etapas que o Copilot executa no navegador para confirmar se o recurso funciona.
3. Leia o relatório e confirme se tudo funciona como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, interromperá o servidor e apresentará um relatório.

## Resumo e próximas etapas

Parabéns! Você usou o servidor Playwright MCP para explorar seu recurso em um navegador real por meio do Copilot CLI. Nesta lição, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionou o servidor Playwright MCP, caso ele ainda não estivesse disponível.
- pediu ao agente que controlasse um navegador e explorasse o recurso de filtragem.

Na próxima lição, você vai [criar um agente personalizado de QA][next-lesson] que combina a skill e as ferramentas do navegador.

## Recursos

- [O que é MCP e por que todos estão falando sobre isso?][mcp-blog-post]
- [Servidor Playwright MCP da Microsoft][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
