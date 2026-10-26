---
title: "Lição 1 - Instalar o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do template, conheça o espaço de trabalho e experimente um chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot][about-copilot-app]** é um aplicativo para desktop voltado ao desenvolvimento orientado por agentes. Ele foi criado com base no GitHub Copilot CLI e tem integração nativa com o GitHub, portanto seus repositórios, branches e pipelines de CI funcionam sem configuração adicional. Ele foi projetado para fluxos de trabalho em que você orienta vários agentes em paralelo — cada um em seu próprio espaço de trabalho isolado — e automatiza tarefas repetitivas, em vez de fazer todo o trabalho por conta própria. Com o Node.js instalado e sua cópia do projeto pronta, a próxima etapa é instalar o aplicativo e conectar esse repositório.

Nesta lição, você vai:

- instalar o aplicativo GitHub Copilot e entrar na sua conta.
- adicionar seu projeto ao aplicativo a partir do repositório do GitHub.
- conhecer o espaço de trabalho, incluindo o backlog criado pelo template.
- experimentar um chat rápido para saber mais sobre o próprio aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para trabalhar em um backlog crescente. O aplicativo Copilot oferece um único lugar para orientar esse trabalho: selecionar issues, executar agentes, revisar alterações e fazer merge de pull requests. Nesta lição, você instalará e conectará o aplicativo e aprenderá a iniciar uma conversa sobre o projeto.

> [!NOTE]
> É necessário ter um plano elegível do Copilot — Copilot Free, Copilot Student ou qualquer plano pago (Pro, Pro+, Business ou Enterprise). Se você usa o Copilot Business ou o Copilot Enterprise, o administrador precisa manter habilitada a política **GitHub Copilot app**.

## Instalar e configurar o aplicativo GitHub Copilot

Para usar o aplicativo GitHub Copilot, primeiro instale-o. Há versões disponíveis para Windows, macOS e Linux. Vamos instalar o aplicativo, autenticar a conta e adicionar o repositório Tailspin Toys.

1. Em um navegador, abra a [página do aplicativo GitHub Copilot][download-app].
2. Baixe o aplicativo para sua plataforma e instale-o seguindo as instruções da página.
3. Abra o aplicativo após a instalação.
4. Selecione **Sign in to GitHub** e siga as instruções para autenticar. Se você usa o GitHub Enterprise Cloud com residência de dados, selecione **Use GitHub Enterprise** e informe seu hostname `*.ghe.com` quando solicitado. O GitHub Enterprise Server não é compatível.
5. Se o processo de configuração oferecer a seleção de repositório, selecione o repositório Tailspin Toys criado a partir do template. Você também pode ignorar essa etapa e adicionar o projeto depois.
6. Escolha um tema quando solicitado e conclua a configuração inicial.
7. Procure seu repositório em **Sessions**, na barra lateral. Se ele já estiver listado, não será necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma nova sessão e, em seguida, selecione **Add GitHub repository**. Use o nome do proprietário e do repositório anotados na lição de pré-requisitos.
9. Confirme que o repositório aparece na barra lateral. No layout usado aqui, pastas de repositório como `tailspin-toys` ficam agrupadas em **Sessions**, com as sessões individuais abaixo de cada repositório.

## Conhecer o espaço de trabalho

Com o projeto conectado, conheça o espaço de trabalho. O aplicativo organiza tudo em algumas áreas na barra lateral:

- **New** — inicie uma nova sessão de chat com o Copilot.
- **Pull requests** — navegue e filtre seus pull requests pela integração nativa com o GitHub, confira o estado da CI e revise PRs sem sair do aplicativo.
- **Issues** — navegue e filtre suas issues e inicie uma sessão a partir de uma issue.
- **Automations** — tarefas de agente salvas que são executadas em uma agenda ou sob demanda. Elas são úteis para listas de tarefas, manutenção regular do projeto e outras atividades repetitivas. A lição de encerramento apresenta automações como próximos passos, não como outro exercício do workshop.
- **Customize** — adicione recursos ao aplicativo Copilot na forma de servidores MCP, plugins, skills e outros componentes. Você usará essa área para configurar o Playwright MCP.
- **Chats** — conversas leves para perguntas e brainstorming que não precisam de uma branch ou espaço de trabalho próprios. Esta seção aparece depois que você inicia seu primeiro chat. Se ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar um chat.
- **Sessions** — agrupa as pastas de repositório e as sessões de agente. Selecione um repositório, como `tailspin-toys`, para abrir sua visualização, ou selecione uma sessão para voltar à conversa. Cada sessão é executada em um espaço de trabalho isolado, então você pode executar várias ao mesmo tempo sem conflitos. Você iniciará sua primeira sessão ao adicionar avaliações em estrelas.

Ao longo do workshop, você explorará o espaço de trabalho.

> [!TIP]
> Na dúvida, pergunte ao Copilot. Se não souber como fazer algo ou se algo é possível, peça orientação ao Copilot.

### Encontrar o backlog inicial

Quase todo projeto tem um backlog, e a Tailspin Toys não é diferente. Vamos explorar o backlog criado quando você criou o repositório a partir do template.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys, não uma sessão individual abaixo dela. Em seguida, selecione **Issues** na navegação superior do repositório.
2. Encontre estas issues pelo título, sem presumir os números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione uma issue para ler os detalhes. Cada issue também pode iniciar uma sessão de agente. Você começará pela issue de filtragem depois de fazer uma primeira alteração rápida.

> [!NOTE]
> As listas em **Issues** e **Pull requests** são filtradas automaticamente para mostrar apenas os itens dos repositórios adicionados ao aplicativo Copilot. Para ver itens de outros repositórios, adicione esses repositórios ao aplicativo.

## Experimentar um chat rápido

Uma ótima maneira de se familiarizar com o aplicativo é usá-lo para saber mais sobre o *próprio aplicativo*. Um **chat rápido** é ideal para isso: você pode fazer uma pergunta ou discutir uma ideia sem criar uma branch ou worktree, então não precisa iniciar uma sessão.

1. Na barra lateral, selecione **+** ao lado de **Chats** para abrir um novo chat. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat; **Chats** aparecerá depois.
2. Pergunte ao aplicativo como funcionam as próprias sessões:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Leia a resposta na visualização da conversa. Você verá que cada sessão é executada em seu próprio git worktree isolado, o que permite executar vários agentes em paralelo sem que as alterações entrem em conflito. Você pode continuar a conversa ou iniciar um novo chat a qualquer momento.

## Resumo e próximas etapas

Parabéns! Você instalou o aplicativo GitHub Copilot, conectou o projeto e explorou o espaço de trabalho. Você aprendeu a:

- instalar o aplicativo e entrar no GitHub.
- adicionar um projeto a partir do repositório do GitHub.
- conhecer o espaço de trabalho e localizar o backlog inicial em **Issues**.
- usar um chat rápido para fazer uma pergunta breve e descartável.

Em seguida, você [iniciará sua primeira sessão de agente][next-lesson] e a usará para exibir uma avaliação em estrelas nos cards dos jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Introdução ao aplicativo GitHub Copilot][getting-started]
- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
