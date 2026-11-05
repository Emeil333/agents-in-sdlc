---
title: "Lição 4 - Oriente o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica, você vai querer que o Copilot tenha acesso a essa orientação. Os [arquivos de instruções][instruction-files] descrevem não apenas *que* código você quer, mas também *como* ele deve ser estruturado. Agora que você criou a filtragem, vai explorar as instruções usadas pelo Copilot, adicionar um padrão de documentação e aplicá-lo ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instruções com escopo por caminho chegam ao agente.
- atualizar o arquivo de instruções para garantir o cumprimento dos padrões de programação.
- observar o impacto dos arquivos de instruções no código.

## Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- Os comentários devem explicar a intenção e decisões não óbvias, em vez de repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar sua finalidade, seus parâmetros e valores de retorno com TSDoc/JSDoc, incluindo um argumento `db` injetável quando aplicável.
- Componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- As orientações de formatação e lint existentes devem ser preservadas.

Com arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar tarefas de acordo com essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot, ajudando-o a compreender seu estilo de programação e seus requisitos. Esse recurso permite orientar as sugestões e os trechos de código para que sejam mais relevantes. Você pode especificar convenções de programação, bibliotecas e os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo, oferecendo contexto no nível da tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações.
- `.github/instructions/*.instructions.md`: arquivos com orientações para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instrução varia conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explore os arquivos de instruções personalizadas deste projeto

O projeto inicial já inclui arquivos de instruções. Vamos explorá-los antes de fazer alterações para observar o impacto.

1. Volte ao Codespace.
2. No editor do Codespaces, abra `.github/copilot-instructions.md`.
3. Explore o arquivo e observe a breve descrição do projeto e as orientações de programação. Essas instruções se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um glob que determina a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos do projeto. Assim, conjuntos maiores de instruções podem ser divididos em arquivos menores e reutilizáveis, além de apontar para exemplos.

## Atualize as instruções para refletir as orientações da equipe

Os arquivos existentes são um bom começo, mas ainda há uma lacuna. Vamos atualizar o arquivo principal `copilot-instructions.md` para garantir que o TypeScript recém-gerado inclua comentários TSDoc.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, que deve estar perto da linha 35.
2. Adicione o seguinte como o último item:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Aplique as orientações atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Depois de salvar as instruções, saia e retome a conversa de filtragem para que o Copilot use as orientações novas sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit`. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme se a branch `game-filters-cli` está selecionada.
3. Peça ao Copilot que aplique as orientações atualizadas:

   ```plaintext
   Acabamos de atualizar as instruções e as orientações de programação. Atualize o código gerado para que siga essas orientações.
   ```

4. Digite `/diff` e leia os arquivos TypeScript alterados. Observe os novos comentários TSDoc e confirme se explicam o código com precisão.

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções e aplicou um novo padrão ao recurso. Nesta lição, você:

- explorou como as instruções do repositório e os arquivos de instruções com escopo por caminho chegam ao agente.
- atualizou o arquivo de instruções para garantir o cumprimento dos padrões de programação.
- observou o impacto dos arquivos de instruções no código.

Na próxima lição, você vai [personalizar e executar a skill reutilizável `quality-checks`][next-lesson] para garantir que o lint e os testes sejam executados de forma consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
