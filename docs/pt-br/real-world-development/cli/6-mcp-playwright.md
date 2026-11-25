---
title: "Lição 6 - Validar a funcionalidade com Playwright MCP"
description: "Confira ou configure o Playwright MCP no Copilot CLI e use-o para explorar a funcionalidade de filtragem em um navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve muito mais do que apenas escrevê-lo. Precisamos trabalhar com dados e serviços externos e, às vezes, oferecer automações adicionais ao Copilot. Para isso, usamos servidores do Model Context Protocol (MCP), que permitem ao Copilot ir além dos recursos integrados ao CLI e acessar outras ferramentas e serviços.

Nesta lição, você vai:

- entender o que é Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionar o servidor MCP do Playwright, se ele ainda não estiver disponível.
- pedir ao agente que controle um navegador e explore a funcionalidade de filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar alterações na interface do usuário (UI) exige interagir com ela. Você quer que o Copilot use o site em que está trabalhando como uma pessoa usuária faria, automatizando ainda mais as alterações e aumentando a confiança de que elas funcionam como esperado.

## O que é Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas e executar ações em seu nome.

As ferramentas e os recursos ficam disponíveis por meio de um servidor MCP, que atua como uma ponte entre o agente de IA e as ferramentas e os serviços externos. Cada servidor oferece ao agente um conjunto diferente de ferramentas e recursos.

Dois servidores MCP populares são:

- **[GitHub MCP Server][github-mcp]**: fornece acesso às APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: oferece automação do navegador com o Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub hospeda um [registro MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique quem o publica e considere as implicações de segurança.

## Adicionar o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária faria.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas e passe para a próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, digite este comando para abrir a caixa de diálogo de adição de servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Pressione <kbd>Enter</kbd> para confirmar o tipo de servidor **STDIO** e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o seguinte na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo MCP.

## Pedir ao Copilot para explorar a funcionalidade com o Playwright

Você já verificou manualmente que a funcionalidade se comporta como esperado. Agora, use o servidor MCP do Playwright disponível para o Copilot para fazer a mesma verificação.

1. Use este prompt para pedir ao Copilot que valide a funcionalidade com o servidor MCP do Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Não é necessário pedir explicitamente ao Copilot que use o servidor MCP: ele normalmente infere qual usar. No entanto, como você sabe qual servidor deve ser usado, especificá-lo pode melhorar a consistência dos resultados e economizar tokens.

2. Observe as etapas exibidas pelo Copilot enquanto ele verifica a funcionalidade no navegador.
3. Leia o relatório e confirme que tudo funciona como esperado.

O Copilot iniciará o servidor, interagirá com o site usando o Playwright, interromperá o servidor e fornecerá um relatório.

## Resumo e próximos passos

Você usou o servidor MCP do Playwright para explorar a funcionalidade em um navegador real a partir do Copilot CLI. Nesta lição, você:

- aprendeu o que é Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionou o servidor MCP do Playwright, caso ele ainda não estivesse disponível.
- pediu ao agente que controlasse um navegador e explorasse a funcionalidade de filtragem.

Em seguida, [crie um agente personalizado de QA][next-lesson] para combinar a habilidade e as ferramentas de navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Playwright da Microsoft][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
