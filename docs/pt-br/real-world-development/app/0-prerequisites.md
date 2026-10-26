---
title: "Lição 0 - Pré-requisitos"
description: "Prepare-se para as lições do aplicativo GitHub Copilot: instale o Node.js para o projeto Tailspin Toys e crie sua própria cópia do repositório a partir do template."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O aplicativo GitHub Copilot é um aplicativo para desktop que funciona como ponto central para o Copilot e o GitHub. Ele oferece acesso rápido a issues e pull requests e permite desenvolver com o GitHub Copilot. Durante este workshop, você trabalhará localmente com o aplicativo Tailspin Toys, criado com Astro, usando o aplicativo GitHub Copilot. Antes de começar, verifique se o Node.js está instalado localmente e depois instale o aplicativo Copilot.

Nesta lição, você vai:

- instalar o Node.js para que os testes do projeto possam ser executados no seu computador.
- criar sua própria cópia do projeto Tailspin Toys a partir do template.

## Instalar o Node.js

Em várias lições, você pedirá a um agente que desenvolva funcionalidades e execute o conjunto de testes do Tailspin Toys localmente. Para isso, é necessário o **[Node.js][nodejs]**, o único runtime exigido pelo projeto. Instale a versão **LTS** atual.

A opção mais simples em todas as plataformas é usar o instalador oficial:

1. No sistema operacional, abra uma janela de terminal usando o Windows Terminal, o Terminal do macOS ou o aplicativo que você costuma usar.
2. Execute os comandos a seguir para verificar as versões instaladas do Node.js e do npm:

   ```shell
   node --version
   npm --version
   ```

3. Se os dois comandos forem executados com sucesso e a versão do Node.js atender aos requisitos do README e do `package.json` do projeto, pule para a próxima seção.

> [!TIP]
> Só é necessário concluir estas etapas se você não tiver o Node instalado ou precisar atualizá-lo.

4. Abra a [página de download do Node.js][node-download].
5. Baixe a versão **LTS** para o seu sistema operacional.
6. Execute o instalador e aceite as opções padrão. No Windows, mantenha a opção **Add to PATH** selecionada.
7. Depois da instalação, abra uma nova janela de terminal.
8. Confirme a instalação na nova janela executando:

   ```bash
   node --version
   npm --version
   ```

9. Você deverá ver as versões instaladas do Node.js e do npm.

> [!IMPORTANT]
> O agente do aplicativo Copilot também precisa conseguir executar `node --version` e `npm --version`. O sucesso no seu terminal habitual não garante que esses comandos estejam disponíveis para o aplicativo. Saia completamente do aplicativo e abra-o novamente depois de instalar o Node.js ou alterar o PATH. Se algum dos comandos continuar falhando em uma sessão, siga as [etapas para solucionar problemas do ambiente da sessão][session-environment] antes de continuar.

> [!IMPORTANT]
> Cada worktree também precisa das dependências do projeto e do Chromium do Playwright para executar verificações E2E. Siga o README do repositório Tailspin Toys ao preparar uma worktree e revise qualquer solicitação de instalação antes de aprová-la.

## Configurar o repositório do laboratório

Você trabalhará na sua própria cópia do projeto Tailspin Toys. Crie-a agora a partir do [repositório de template][template-repository]. O novo repositório contém todos os arquivos necessários para o laboratório e será conectado ao aplicativo durante a instalação.

1. Em uma nova janela do navegador, acesse o repositório do GitHub deste laboratório: `https://github.com/github-samples/tailspin-toys`.
2. Crie sua própria cópia do repositório selecionando **Use this template** na página do repositório do laboratório. Em seguida, selecione **Create a new repository**.

   ![Botão Use this template com Create a new repository selecionado no menu suspenso](../../../_images/app-0-use-template.png)

3. Se você estiver participando de um evento organizado pelo GitHub ou pela Microsoft, siga as instruções dos mentores. Caso contrário, crie o repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

   ![Formulário Create a new repository com github-samples/tailspin-toys definido como template e o nome do repositório preenchido](../../../_images/app-0-create-repository.png)

4. Anote o caminho do repositório que você criou (**organization-or-user-name/repository-name**), pois você precisará dele mais adiante no laboratório.

> [!NOTE]
> Quando você cria o repositório a partir do template, um backlog de issues do GitHub é criado automaticamente. Você trabalhará nessas issues durante o workshop — não é necessário registrar nenhuma por conta própria.

Use uma cópia nova do template do workshop. Ela inclui instruções do repositório, código do aplicativo, testes, uma skill `quality-checks` e uma extensão de canvas existente. Durante o workshop, você personalizará a skill e criará um agente de QA. Se estiver usando uma cópia mais antiga, confirme com a pessoa facilitadora se ela inclui os arquivos necessários.

## Resumo e próximas etapas

O ambiente está pronto. Nesta lição, você:

- instalou o Node.js para que o projeto possa ser compilado e testado no seu computador.
- criou sua própria cópia do repositório Tailspin Toys a partir do template.

Em seguida, você [instalará o aplicativo GitHub Copilot][next-lesson], conectará o repositório que acabou de criar e conhecerá o espaço de trabalho.

## Recursos

- [Baixar o Node.js][node-download]
- [Criar um repositório a partir de um template][template-repository]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#iniciar-uma-sessao-a-partir-da-issue
