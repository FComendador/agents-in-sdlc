---
title: "Lição 3 - Modos do agente: Plan e Autopilot"
description: "Use Plan para combinar uma abordagem, Autopilot para implementar a filtragem a partir de uma issue e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando uma pequena funcionalidade ao projeto. Alterações maiores exigem um processo mais robusto. O GitHub Copilot CLI pode se integrar ao fluxo de trabalho de uma organização para ajudar a desenvolver o que é necessário da maneira adequada. Nesta lição e nas seguintes, você seguirá um processo comum de desenvolvimento com agentes: começar por uma issue, criar uma funcionalidade, verificar se o código é válido e se comporta como esperado e, por fim, fazer merge no projeto.

> [!NOTE]
> Você continuará o fluxo de trabalho da funcionalidade usando a mesma conversa e branch. Normalmente, seriam usadas branches ou PRs diferentes para tipos distintos de arquivo, mas simplificaremos algumas etapas para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma nova conversa do Copilot a partir de uma issue do GitHub.
- definir os requisitos no modo Plan.
- implementar a funcionalidade com o modo Autopilot.
- revisar o código.
- validar manualmente a funcionalidade em um navegador encaminhado.

Nas lições seguintes, você atualizará as instruções do repositório, personalizará a habilidade existente `quality-checks`, adicionará validação MCP, criará um agente de QA e abrirá a PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar jogos por categoria e publicadora. A issue do backlog descreve a funcionalidade, mas detalhes como a combinação de categorias precisam ser definidos antes de escrever o código. Você usará o Plan para resolver essas decisões e, depois, autorizará uma implementação delimitada com o Autopilot.

## Contexto

Adicionar agentes de programação com IA ao fluxo de desenvolvimento não altera os fundamentos; na verdade, torna-os ainda mais importantes. A maioria dos desenvolvedores segue um processo semelhante a este:

1. Começar por uma issue registrada que descreva o trabalho.
2. Criar um plano do que precisa ser desenvolvido.
3. Escrever e revisar o código.
4. Executar testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar uma pull request (PR).
7. Após a revisão do código e a aprovação das verificações de integração contínua (CI), fazer merge das alterações.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas a maioria dos processos são variações dessa abordagem.

Esse processo padrão ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelas mesmas revisões que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você trabalha com o agente. Ele sugere alterações e aguarda sua resposta antes de continuar.
- **Plan**: primeiro, cria um plano e não pode editar os arquivos do projeto.
- **Autopilot**: trabalha de forma autônoma, escrevendo código, executando testes e iterando até concluir a tarefa.

Comece no Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Começar pela issue

Antes de trabalhar na filtragem, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o.
2. Confirme que a PR de classificação foi mesclada.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para a filtragem:

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
8. Use as teclas de seta para destacar **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

O prompt agora começa com `#7` ou um número semelhante. `#` permite adicionar uma issue ou PR do GitHub ao contexto.

## Planejar a funcionalidade de filtragem

O planejamento ajuda a decidir como implementar uma funcionalidade ou executar uma tarefa antes de delegá-la ao Copilot. É recomendável dedicar tempo ao planejamento de tarefas complexas. Mude para o modo Plan e peça que o Copilot crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt exibe **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot explorará o projeto e determinará a melhor abordagem para elaborar o plano.

3. O Copilot pode perguntar como a filtragem deve funcionar. Responda de acordo com suas preferências; não há respostas incorretas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot que revise qualquer parte que não corresponda às decisões tomadas.

## Aprovar o Autopilot

Você criou e revisou o plano. Agora, implemente-o com o Autopilot.

O Autopilot permite que o Copilot itere até considerar que a tarefa foi concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na sua versão.
2. Confirme que o indicador abaixo do prompt exibe **Autopilot**.
3. Observe o Copilot seguir o plano, gerar código e executar testes.

> [!NOTE]
> A implementação pode começar assim que você aprovar o plano; revise-o antes. Se o Copilot informar que há dependências ausentes ou conflitos de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e validar a implementação

Assim como qualquer outro código, o código gerado precisa ser revisado antes do merge. Você revisará o código e executará o site para verificar se tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Interactive. Confirme que o indicador não exibe **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e as decisões do planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização diff.
5. Confira os resultados das verificações do projeto e peça ao Copilot que corrija as falhas.

## Explorar a nova funcionalidade

O código parece correto, mas será que é executado? Inicie o aplicativo como antes e abra o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces indicar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros de categoria, os de publicadora e as combinações definidas no plano.
4. Confirme que a redefinição dos filtros e o estado sem resultados correspondem à issue e às decisões tomadas.
5. Volte ao Codespace e peça ao Copilot para interromper o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma nova conversa do Copilot a partir de uma issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou a funcionalidade com o modo Autopilot.
- revisou o código.
- validou manualmente a funcionalidade em um navegador encaminhado.

Em seguida, [use instruções personalizadas][next-lesson] para se aprofundar na geração de código de acordo com as práticas documentadas.

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
