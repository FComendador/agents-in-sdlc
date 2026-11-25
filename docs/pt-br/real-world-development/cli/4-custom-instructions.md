---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica, garanta que o Copilot possa acessar essas orientações. Os [arquivos de instruções][instruction-files] descrevem não apenas *o que* você quer no código, mas também *como* ele deve ser estruturado. Agora que você criou a filtragem, vai revisar as instruções usadas pelo Copilot, adicionar um padrão de documentação e aplicá-lo ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos aplicáveis a caminhos específicos chegam ao agente.
- atualizar o arquivo de instruções para garantir que os padrões de codificação sejam seguidos.
- observar o efeito dos arquivos de instruções no código.

## Cenário

A Tailspin Toys tem diretrizes e requisitos para suas práticas de desenvolvimento:

- Os comentários devem explicar a intenção e decisões não óbvias, e não repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar sua finalidade, parâmetros e valores de retorno com TSDoc/JSDoc, incluindo o argumento `db` injetável, quando houver.
- Os componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários devem ser atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser preservadas.

Com os arquivos de instruções, você dará ao Copilot o contexto necessário para seguir essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot para ajudá-lo a entender seu estilo e seus requisitos. É possível especificar convenções, bibliotecas e tipos de comentários. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md` é um arquivo único enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações sobre o projeto relevantes para a maioria delas.
- `.github/instructions/*.instructions.md` contém orientações para linguagens, tipos de arquivo ou tarefas específicos.

> [!NOTE]
> Os formatos de instrução e o suporte variam de acordo com o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explorar os arquivos de instruções personalizadas do projeto

O projeto inicial já inclui vários arquivos de instruções. Explore-os antes de fazer alterações para observar como funcionam.

1. Volte ao Codespace.
2. No editor do Codespaces, não no terminal, abra `.github/copilot-instructions.md`.
3. Examine a breve descrição do projeto e as orientações de codificação. Elas se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define o glob que determina a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos existentes do projeto. Assim, é possível dividir conjuntos grandes de instruções em arquivos menores e reutilizáveis, além de indicar exemplos que o Copilot deve seguir.

## Atualizar os arquivos de instruções de acordo com as diretrizes da equipe

Os arquivos existentes são um bom começo, mas falta uma regra. Edite o arquivo principal `copilot-instructions.md` para exigir comentários TSDoc no novo código TypeScript.

1. Em `.github/copilot-instructions.md`, procure a seção **Code formatting guidance**, que deve estar perto da linha 35.
2. Adicione este item ao final da lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Aplicar as instruções atualizadas

O Copilot CLI carrega as instruções do repositório ao iniciar uma conversa. Depois de salvar as instruções, encerre e retome a conversa de filtragem para que o Copilot carregue as novas orientações sem perder o contexto da funcionalidade.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para encerrá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está selecionada.
3. Peça ao Copilot para aplicar as orientações atualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e revise os arquivos TypeScript alterados. Observe os novos comentários TSDoc e confirme que descrevem o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot CLI obtém contexto dos arquivos de instruções e aplicou um novo padrão à funcionalidade. Nesta lição, você:

- explorou como as instruções do repositório e os arquivos aplicáveis a caminhos específicos chegam ao agente.
- atualizou o arquivo de instruções para garantir que os padrões de codificação sejam seguidos.
- observou o efeito dos arquivos de instruções no código.

Em seguida, [personalize e execute a habilidade reutilizável `quality-checks`][next-lesson] para aplicar as mesmas verificações de lint e testes.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot: coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
