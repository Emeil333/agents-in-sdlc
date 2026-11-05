---
title: "Lição 3 - Modos do agente: Plan e Autopilot"
description: "Use o Plan para combinar uma abordagem, o Autopilot para implementar a filtragem a partir de uma issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. Alterações maiores, porém, exigem um processo mais completo. Felizmente, o GitHub Copilot CLI foi criado para funcionar com o fluxo de trabalho existente da organização e ajudar a desenvolver as soluções certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento orientado por agentes: começará com uma issue para criar um recurso, verificará o código e o comportamento e, por fim, fará o merge no projeto.

> [!NOTE]
> Você continuará o fluxo de trabalho do recurso usando a mesma conversa e branch. Normalmente, seriam usadas branches ou PRs diferentes para cada tipo de arquivo, mas este atalho ajuda a manter o foco nos conceitos principais.

Nesta lição, você vai:

- iniciar uma nova conversa do Copilot a partir de uma issue do GitHub.
- definir os requisitos no modo Plan.
- implementar o recurso com o modo Autopilot.
- revisar o código.
- validar manualmente o recurso em um navegador encaminhado.

Ao continuar este fluxo do recurso, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará a validação por MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo do Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue da lista de pendências descreve o recurso, mas decisões como combinar categorias precisam ser acordadas antes de escrever o código. Você usará o modo Plan para resolver essas questões e, depois, autorizará uma implementação com escopo definido pelo Autopilot.

## Contexto

A introdução de agentes de programação com IA ao fluxo de desenvolvimento não altera os fundamentos; na verdade, torna-os ainda mais importantes. O processo de muitos desenvolvedores se parece com este:

1. Começar com uma issue criada que descreva o que precisa ser feito.
2. Planejar o que será desenvolvido.
3. Criar e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Fazer o merge do código depois que ele for revisado e o processo de integração contínua for concluído com êxito.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria dos processos segue uma variação dessas etapas.

Seguir essa abordagem ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua resposta antes de continuar.
- **Plan**: o agente cria primeiro um plano e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escrevendo código, executando testes e iterando até concluir a tarefa.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Comece pela issue

Antes de começar a filtragem, volte ao Codespace e confirme se o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme se o PR de avaliação por estrelas foi integrado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as teclas de seta para destacar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt começa com `#7` (ou um número semelhante). O caractere `#` permite incluir uma issue ou um pull request (PR) do GitHub no contexto.

## Planeje o recurso de filtragem

O planejamento permite definir a abordagem para implementar um recurso ou realizar uma tarefa antes de delegá-la ao Copilot. Para tarefas complexas, vale a pena reservar um tempo para planejar. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme se o indicador abaixo do prompt exibe **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite este prompt:

   ```plaintext
   Crie um plano para implementar esse recurso.
   ```

O Copilot começará a trabalhar no plano. Primeiro, explorará o projeto e depois determinará a melhor abordagem.

3. Durante o processo, o Copilot poderá fazer perguntas sobre o funcionamento da filtragem. Responda de acordo com suas preferências; não há respostas erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot que revise qualquer parte que não corresponda às suas decisões.

## Aprove o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Vamos deixar o Copilot trabalhar usando o modo Autopilot.

O Autopilot permite que o Copilot itere até considerar a tarefa concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na versão instalada.
2. Confirme se o indicador abaixo do prompt exibe **Autopilot**.
3. Observe o Copilot seguir o plano, gerar o código e executar os testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Depois que o código for gerado, revise-o antes de fazer o merge, como faria com qualquer outro código. Vamos revisar o código e executar o site para confirmar se tudo está funcionando.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme se o indicador do modo não exibe mais **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões tomadas durante o planejamento.
4. Depois da revisão, pressione <kbd>Esc</kbd> para sair da visualização de diferenças.
5. Revise o resultado das verificações do projeto e peça ao Copilot que corrija as falhas.

## Explore a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa testar o recurso de filtragem no navegador. Informe a URL e deixe o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Experimente filtrar por categoria, por editora e pelas combinações definidas no plano.
4. Confirme se o comportamento de redefinição e de resultados vazios corresponde à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que interrompa o servidor de desenvolvimento iniciado.

## Resumo e próximas etapas

Você usou diferentes modos de conversa para criar e revisar um recurso. Nesta lição, você:

- iniciou uma nova conversa do Copilot a partir de uma issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou o recurso com o modo Autopilot.
- revisou o código.
- validou manualmente o recurso em um navegador encaminhado.

Na próxima lição, você vai entender melhor como gerar código que siga práticas documentadas ao [usar instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
