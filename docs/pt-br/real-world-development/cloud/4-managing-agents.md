---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos dois exercícios anteriores, você pediu ao agente de nuvem do GitHub Copilot que executasse três tarefas para melhorar a experiência de usuário e adicionar funcionalidades. Embora o agente trabalhe de forma autônoma e assíncrona, ainda é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse centro de controle, você vê todas as tarefas do agente com pull requests abertos, examina as operações realizadas e orienta sessões em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar tarefas do agente de nuvem.
- orientar uma sessão em andamento para pedir funcionalidades adicionais.

## Cenário

Depois de atribuir ao agente a criação do modo de alto contraste, a equipe percebeu que também seria um bom momento para adicionar um modo claro. Como o trabalho já incluía atualizar o estilo do site e adicionar a funcionalidade de alternância, fazia sentido incluir esse recurso. Você vai orientar o agente para garantir que ele adicione também o modo claro.

## Revise as tarefas do agente de nuvem do GitHub Copilot

Vamos verificar o estado atual das tarefas atribuídas ao agente.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as listas de tarefas no painel principal e no painel à esquerda. Você verá as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em execução e revise as operações realizadas pelo Copilot. Elas podem incluir:
   - fazer checkout do código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exatas exibidas variam de acordo com o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull requests à direita. Ele permite ver o pull request e os arquivos alterados para acompanhar o trabalho.

## Oriente o agente de nuvem

Agora que você viu as tarefas ativas, peça ao Copilot para incluir o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à adição do modo de alto contraste. O título exato depende do nome escolhido pelo Copilot e do estado do trabalho.

    ![Sessão de acessibilidade no centro de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você verá mensagens semelhantes às seguintes.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o prompt a seguir:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para orientar a sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o pedido e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot continuará com a tarefa atualizada. Ele incorporará o novo pedido ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

Isso levará alguns minutos. Aproveite a pausa para refletir sobre o que você aprendeu e explorou até agora.

## Resumo e próximos passos

Você explorou a página de agentes do Copilot, o ponto central para monitorar e orientar tarefas do agente de nuvem do GitHub Copilot. Nesse centro de controle, é possível acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar sessões em andamento para alterar o escopo ou fornecer mais orientações.

Você explorou estes conceitos:

- explorou o centro de controle e a página de agentes para monitorar tarefas do agente de nuvem.
- redirecionou uma sessão em andamento para pedir funcionalidades adicionais.

Enquanto o Copilot conclui os recursos de acessibilidade, você pode [iterar nos pull requests criados por ele][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
