---
title: "Lição 2 - Adicionar classificação por estrelas: uma melhoria rápida"
description: "Use o Copilot CLI para exibir classificações nos cards de jogos, confira a alteração em um navegador encaminhado e faça merge da sua primeira pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você instalou o Copilot CLI e experimentou uma conversa. Agora, fará sua primeira alteração no projeto. Ela será pequena: os dados dos jogos já incluem uma classificação, mas ela ainda não aparece nos cards da página inicial. Você pedirá ao agente que exiba a classificação existente em cada card, analisará a alteração e fará merge como sua primeira pull request.

Nesta lição, você vai:

- iniciar uma conversa do Copilot focada em uma funcionalidade, a partir de uma branch.
- pedir ao agente que faça uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- executar o aplicativo e verificar a alteração em um navegador encaminhado.
- abrir e fazer merge da sua primeira pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma classificação por estrelas, que já aparece na página de detalhes. No entanto, os cards da página inicial exibem somente o título, a categoria, a publicadora e a descrição. Para começar, você pedirá ao agente que mostre a classificação existente em cada card: uma alteração pequena e bem delimitada, ideal para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é o espaço em que você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal do CLI usa o repositório e a branch Git atuais do terminal, em vez de criar um worktree dedicado. Você pode retornar a conversas salvas; os arquivos e a branch continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá três elementos: seus prompts e as respostas do agente, a atividade das ferramentas enquanto ele explora e edita arquivos e as alterações que você pode revisar com `/diff`.

## Iniciar uma conversa e solicitar a alteração

Inicie uma conversa nova para implementar a funcionalidade.

1. Retorne ao seu Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot CLI ainda não estiver em execução, use:

   ```bash
   copilot --yolo
   ```

4. Digite o comando de barra `/new` e pressione <kbd>Enter</kbd> para iniciar uma sessão nova.
5. Solicite a alteração com este prompt:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot vai explorar o projeto, localizar os arquivos que exibem os detalhes dos jogos e criar o código necessário. Você adicionou uma funcionalidade com o Copilot CLI.

## Revisar o diff

Antes de fazer merge, é necessário revisar todas as alterações geradas por IA, mesmo as pequenas. Examine a alteração no Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme que o card mostra a classificação numérica quando ela existe e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois estados.
4. Confira os resultados das verificações executadas pelo Copilot e peça que ele corrija as falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da visualização diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico; por isso, o código exato pode variar. Verifique o comportamento em vez de esperar uma implementação idêntica.

## Verificar as alterações

Não basta ler o código e presumir que ele funciona. Peça ao Copilot para iniciar o site e confira a interface do usuário (UI) atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces indicar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cards exibem as classificações de cinco estrelas.
4. Volte ao Copilot e peça que ele pare o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abrir e fazer merge da sua primeira pull request

Você criou a funcionalidade. Agora, criará uma pull request (PR) para fazer merge do novo código no projeto.

1. Peça ao agente padrão para confirmar a alteração:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. O Copilot CLI enviará o commit existente ao criar a PR e exibirá a URL.
3. Mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione a URL exibida pelo Copilot CLI para abrir a PR.
4. Revise os arquivos alterados e as verificações.
5. Quando tudo estiver pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração com o GitHub Copilot CLI. Nesta lição, você:

- iniciou uma conversa do Copilot focada em uma funcionalidade, a partir de uma branch.
- pediu ao agente que fizesse uma pequena alteração nos cards de jogos.
- revisou a alteração com `/diff`.
- executou o aplicativo e conferiu a classificação em um navegador encaminhado.
- abriu e fez merge da sua primeira pull request.

Em seguida, [comece pela issue de filtragem e use os modos Plan e Autopilot][next-lesson] para desenvolver uma funcionalidade maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
