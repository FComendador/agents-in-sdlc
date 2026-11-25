---
title: "Lição 8 - Criar e fazer merge da PR da funcionalidade"
description: "Revise em conjunto a filtragem, as instruções, a habilidade, o perfil de QA e os testes; depois crie uma PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções, a atualização da habilidade, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Está na hora de revisar tudo em conjunto e abrir uma pull request. Você fez merge manual da PR de classificação por estrelas; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos a funcionalidade, as atualizações das instruções, a habilidade e o agente de QA em algumas PRs separadas. Para simplificar o workshop, você manteve todo o trabalho de filtragem e qualidade em uma conversa e branch e incluirá tudo nesta PR.

Nesta lição, você vai:

- entender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionar a alteração completa da funcionalidade e as evidências de validação.
- criar a PR de filtragem.
- ativar o Agent Merge somente depois da revisão e confirmar que a PR foi mesclada.

## Cenário

Durante o fluxo de trabalho da filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora, a Tailspin Toys quer automatizar o trabalho restante da PR, mantendo a autorização do merge sob o controle do desenvolvedor.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para concluir uma pull request. Quando ativado, o Copilot resolve o que está impedindo a PR — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e, depois, ativa o merge automático do GitHub se o repositório permitir.

Até agora, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de ativar o Agent Merge.

## Usar o Agent Merge para gerenciar a PR

Com todo o código criado, vamos revisar as alterações, criar a PR e deixar o Agent Merge gerenciar o restante do processo.

1. Volte ao Codespace.
2. Digite `/agent` para abrir a caixa de diálogo de agentes.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie uma nova PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua da PR. Quando tudo for aprovado, ele fará o merge.
7. Confirme que o Copilot exibiu uma mensagem semelhante a `"PR #14 was squash-merged successfully."`.

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório ou permissões ausentes. Se houver um bloqueio, leia o motivo informado e conclua manualmente o merge revisado quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias partes do processo de desenvolvimento: geração de código, testes e validação e, agora, o processo da pull request. Nesta lição, você:

- entendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionou a alteração completa da funcionalidade e as evidências de validação.
- criou a PR de filtragem.
- ativou o Agent Merge somente depois da revisão e confirmou que a PR foi mesclada.

Em seguida, [explore outros comandos de barra úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional ao agente na nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
