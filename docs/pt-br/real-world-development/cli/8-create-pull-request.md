---
title: "Lição 8 - Crie e faça o merge do PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma branch. É hora de revisar tudo em conjunto e abrir um pull request. Você fez o merge do PR de avaliação por estrelas manualmente; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, você manteve todo o fluxo de filtragem e qualidade em uma conversa e branch, e incluirá tudo neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionar todas as alterações do recurso e as evidências de validação.
- criar o PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar o merge do PR.

## Cenário

Durante o fluxo de trabalho de filtragem, você usou o Copilot para planejar, implementar e validar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização do merge sob o controle do desenvolvedor.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para concluir um pull request. Quando habilitado, o Copilot resolve os itens que estão bloqueando o PR — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e atualiza a branch quando necessário — e depois habilita o merge automático do GitHub, se o repositório permitir.

Até agora, você mesmo selecionou **Merge pull request**. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que ele está pronto antes de habilitar o Agent Merge.

## Use o Agent Merge para gerenciar o PR

Com todo o código pronto, vamos revisá-lo em conjunto, criar o PR e deixar o Agent Merge gerenciar o restante do processo.

1. Volte ao Codespace.
2. Abra a caixa de diálogo de agente digitando `/agent`.
3. Na lista de opções, selecione **Default** e pressione <kbd>Enter</kbd>.
4. Crie um novo PR usando o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua do PR. Quando tudo passar, ele fará o merge.
7. Confirme se o Copilot exibe uma mensagem semelhante a "PR #14 was squash-merged successfully".

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branches, filas de merge, configurações do repositório nem permissões ausentes. Se o processo for bloqueado, leia o motivo informado e, quando o repositório permitir, faça manualmente o merge que você já revisou.

## Resumo e próximas etapas

Você automatizou várias partes do processo de desenvolvimento: geração de código, testes e validação e, agora, o processo do pull request. Nesta lição, você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionou todas as alterações do recurso e as evidências de validação.
- criou o PR de filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou o merge do PR.

Na próxima lição, você vai [explorar outros comandos de barra úteis do Copilot CLI][next-lesson] para gerenciar o contexto, os modelos e o compartilhamento, além da delegação opcional à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
