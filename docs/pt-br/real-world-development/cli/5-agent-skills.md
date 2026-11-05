---
title: "Lição 5 - Personalize e use uma skill quality-checks"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escrever código envolve mais do que apenas escrever código. Já conseguimos validar manualmente se o código funciona e usar arquivos de instruções para garantir que ele siga nossos padrões. Mas e os testes? E o lint? E todas as outras partes da integração contínua (CI)?

Para esse tipo de tarefa, as **skills de agente** são a melhor opção! As skills ajudam o Copilot a entender como executar corretamente operações como essas.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente.
- personalizar o formato dos resultados.
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes unitários, lint e verificações de tipos. A equipe quer melhorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente reúnem instruções reutilizáveis para tarefas, scripts executáveis e recursos de apoio que um agente carrega sob demanda. Em sua forma básica, são pastas com o nome da skill e um arquivo Markdown chamado `SKILL.md`. O Markdown contém um frontmatter com nome e descrição para definir a skill, uma visão geral do que ela faz e orientações sobre quando deve ser chamada. A pasta também pode incluir subpastas com scripts e outros recursos que a skill usa quando é chamada.

> [!NOTE]
> Pastas e arquivos adicionais não são necessários para uma skill. A skill `quality-checks` da Tailspin Toys contém apenas `SKILL.md`, pois usa os comandos existentes do projeto.

As skills podem ficar na pasta `.github/skills` do projeto para se tornarem um recurso do repositório, compartilhado e reutilizado pela equipe, ou na pasta de skills do usuário em `~/.copilot/skills`.

## Explore a skill

Vamos explorar a skill que a equipe da Tailspin Toys criou para executar testes unitários, lint e verificações de tipos, chamada `quality-checks`.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia os campos `name` e `description` no início do arquivo. A descrição ajuda o Copilot a entender quando chamar a skill.
3. Leia as instruções e observe como elas orientam o Copilot durante os testes e o lint.
4. Observe que a skill ainda não tem uma seção **Formato do relatório de resultados**.

## Execute a skill antes de fazer alterações

As skills podem ser chamadas diretamente pelo Copilot CLI ou usando linguagem natural. Vamos pedir ao Copilot que execute as três verificações da skill.

1. Volte à conversa sobre filtragem no modo Interactive.
2. Use o seguinte prompt:

   ```plaintext
   Execute a skill `quality-checks` para testes unitários, lint e verificações de tipos.
   ```

3. Observe o relatório final.

## Personalize o relatório

Agora, queremos um relatório melhor que informe o que foi executado, se houve aprovação ou falha e o que as ferramentas relataram. Vamos atualizar a skill para gerar esse relatório.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seguinte seção ao fim do arquivo:

   ```markdown
   ## Formato do relatório de resultados

   Ao concluir, informe cada comando executado e se ele foi aprovado, falhou ou foi bloqueado. Inclua contagens de testes, durações, erros, avisos e outras métricas somente quando a ferramenta as relatar. Indique a próxima ação para cada falha ou bloqueio e nunca descreva uma verificação ignorada ou incompleta como aprovada.
   ```

3. O arquivo é salvo automaticamente.

## Execute a skill atualizada

Depois de fazer a alteração, vamos ver a skill em ação! O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use exatamente o mesmo prompt de antes:

   ```plaintext
   Execute a skill `quality-checks` para testes unitários, lint e verificações de tipos.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes unitários, lint e verificações de tipos.
- personalizou o formato dos resultados.
- recarregou e executou a skill.

Essa alteração acompanhará a filtragem no PR do recurso. A seguir, você permitirá que o Copilot interaja diretamente com o site [por meio do servidor MCP do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade são referências, não tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e um script de exportação incluído: `drawio`][drawio-example]
- [Testes no navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
