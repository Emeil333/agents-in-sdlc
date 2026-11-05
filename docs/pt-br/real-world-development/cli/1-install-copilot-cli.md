---
title: "Lição 1 - Instale o GitHub Copilot CLI"
description: "Instale e autentique o Copilot CLI no Codespace, conheça o ambiente e encontre a issue de filtragem já criada."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O [GitHub Copilot CLI][about-copilot-cli] é um assistente de programação com agentes que funciona no terminal. Ele permite explorar bases de código, gerar código, executar comandos e interagir com ferramentas externas pela linha de comando. Assim, você pode delegar tarefas, pedir alterações e manter o foco. O primeiro passo, como você já deve imaginar, é instalar a ferramenta. Felizmente, isso pode ser feito com ferramentas que você já conhece.

Nesta lição, você vai:

- instalar o GitHub Copilot CLI com npm.
- autenticar-se com sua conta do GitHub.
- confiar no repositório do workshop e iniciar uma conversa rápida.
- encontrar a issue de filtragem pelo servidor GitHub MCP integrado.

## Cenário

Sua equipe está começando a usar agentes de IA para trabalhar em uma lista de pendências crescente. O Copilot CLI leva esses recursos ao terminal, onde muitos desenvolvedores já trabalham. Esta lição instala e autentica a ferramenta para que você possa usá-la no restante do workshop.

## Instale o Copilot CLI

Você pode instalar o Copilot CLI com [npm][install-cli], WinGet ou Homebrew. Como o GitHub Codespaces já inclui o Node.js, você usará npm.

1. Volte ao Codespace e abra um terminal.
2. Confirme que o Node.js está instalado e atende ao requisito de versão:

   ```bash
   node --version
   ```

   A versão exibida deve ser 24 ou superior.

3. Instale o Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Verifique a instalação:

   ```bash
   copilot --version
   ```

   O número da versão será exibido.

## Autentique-se no GitHub

Na primeira inicialização, o Copilot CLI solicita que você se autentique com sua conta do GitHub.

1. Inicie o Copilot CLI:

   ```bash
   copilot
   ```

2. Se solicitado, siga as instruções do código do dispositivo para autenticar-se e autorizar o Copilot CLI.
3. O Copilot CLI exibe o seguinte prompt:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirme se o caminho corresponde ao seu repositório Tailspin Toys e responda que sim selecionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> Em um Codespace, talvez você já esteja autenticado pela sua sessão do GitHub. Se o Copilot CLI iniciar sem pedir autenticação, está tudo certo.

## Conheça o ambiente

Os comandos no prompt normal do shell são executados diretamente no Codespace. Depois que o Copilot CLI é iniciado, a linguagem natural é enviada ao agente, enquanto os comandos de barra controlam a conversa.

1. Digite `/model`, use as teclas de seta para selecionar **Auto**, pressione <kbd>Enter</kbd> e pressione <kbd>Enter</kbd> novamente para confirmar.
2. Digite `/help` para ver os comandos disponíveis na versão instalada e pressione <kbd>Esc</kbd> para fechar a tela de ajuda.
3. Faça uma pergunta simples ao Copilot para confirmar que tudo está funcionando:

   ```plaintext
   Quais são os principais arquivos deste projeto?
   ```

4. Leia a resposta e observe como o Copilot explora o repositório antes de responder.
5. Digite `/mcp list` e confirme se o servidor GitHub MCP integrado está disponível.
6. Peça ao Copilot para localizar a issue de filtragem:

   ```plaintext
   Usando o GitHub MCP, encontre neste repositório a issue intitulada "Allow users to filter games by category and publisher". Informe a URL e um breve resumo. Não faça nenhuma alteração.
   ```

7. Abra a URL e leia a issue. Você a usará depois de fazer uma primeira alteração rápida.

> [!TIP]
> Uma sessão normal do Copilot CLI funciona na branch que está atualmente selecionada no terminal; ela não cria um worktree automaticamente. Você criará uma branch de funcionalidade antes de cada alteração.

## Use o atalho do workshop

Normalmente, o Copilot CLI pede aprovação antes de usar ferramentas fora das permissões já concedidas. Neste workshop, você vai reiniciá-lo com `--yolo`, um atalho autorizado pelo usuário que remove esses pedidos de aprovação dentro do Codespace para que você possa se concentrar nos exercícios.

> [!CAUTION]
> `--yolo` habilita permissões totalmente automáticas (`--allow-all-tools`, `--allow-all-paths` e `--allow-all-urls`). Use essa opção somente em um ambiente isolado, como um Codespace ou uma VM, e nunca a defina como padrão para o desenvolvimento diário. Consulte [Permitir e negar o uso de ferramentas][allow-all-warning] para saber mais.

Neste workshop, `--enable-all-github-mcp-tools` habilita as ferramentas GitHub MCP de leitura e gravação usadas nas lições posteriores para trabalhar com issues e pull requests. O Codespace limita o acesso ao computador local, mas os recursos autenticados do GitHub continuam sendo reais. Revise as alterações antes de publicá-las ou fazer o merge.

1. Saia do Copilot CLI com `/exit`.
2. Reinicie-o na raiz do repositório:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Faça outra pergunta rápida sobre o projeto para confirmar que a conversa está funcionando e, em seguida, saia com `/exit`.

O Copilot salva as conversas automaticamente. Mais adiante, depois de alterar as instruções ou adicionar um agente, você usará `copilot --resume` para voltar à mesma conversa de funcionalidade e à mesma branch.

## Resumo e próximas etapas

Parabéns! Nesta lição, você:

- instalou o GitHub Copilot CLI usando npm.
- autenticou-se com sua conta do GitHub.
- confiou no repositório do workshop e iniciou uma conversa rápida.
- encontrou a issue de filtragem pelo servidor GitHub MCP integrado.

Na próxima lição, você vai [iniciar sua primeira alteração focada][next-lesson] e usar o Copilot CLI para exibir uma avaliação por estrelas nos cartões dos jogos.

## Recursos

- [Instalar o GitHub Copilot CLI][install-cli]
- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
