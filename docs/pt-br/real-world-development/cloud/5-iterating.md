---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse uma funcionalidade de jogos relacionados para a equipe de design iterar e implementasse recursos de acessibilidade, incluindo controles para modo de alto contraste e modo claro. Vamos explorar as alterações de código e, se necessário, dar feedback ao Copilot antes de habilitar as revisões da equipe.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a introdução da IA generativa. Sempre devemos revisar o código gerado e seguir o processo DevOps habitual. Vamos revisar as sugestões do GitHub Copilot para criar documentação, implementar jogos relacionados e adicionar recursos de acessibilidade antes de solicitar a revisão da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para manter o trabalho seguro:

- O Copilot tem acesso de leitura ao repositório e acesso de escrita **somente** à branch usada para o código.
- O agente de nuvem é executado no GitHub Actions, em um ambiente separado e temporário.
- Os fluxos de trabalho do GitHub Actions precisam de aprovação humana antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revisar a documentação gerada

Vamos começar explorando o primeiro pull request (PR) gerado pelo agente de nuvem do GitHub Copilot: a inclusão de documentação no código. Você fará isso usando a interface padrão de PR do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você poderá ver um aviso de que o firewall bloqueou o GitHub Copilot. Isso é **esperado**, pois o acesso do Copilot a recursos externos — incluindo servidores MCP externos — é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao repositório em github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui novos comentários TSDoc e outras documentações. As alterações exatas variam.

   Durante a revisão, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Eles são gerados pelas instruções personalizadas revisadas no início do Exercício 2.

6. Depois de revisar as atualizações, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador informando que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os fluxos deverão ser enfileirados na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você pode mencionar o Copilot em comentários do PR ou em comentários em linha no código, como faria com outras pessoas da equipe. O Copilot verá os comentários e iniciará outra sessão para atendê-los. Como os resultados são não determinísticos, não há um texto único que você precise usar. Você pode pedir, por exemplo:

- adicionar cabeçalhos de comentário no início de cada arquivo de código, com uma breve descrição.
- adicionar comentários TSDoc a arquivos TypeScript e Astro.
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das sugestões acima ou outra ideia de documentação para a base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para aplicar as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Depois que o Copilot concluir as alterações, um novo commit aparecerá no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você poderá marcar o PR como pronto para revisão e fazer merge na branch principal.

![Marcar PR como pronto](../../../_images/ex7-ready-for-review.png)

## Revisar a funcionalidade de jogos relacionados

Agora, volte ao PR criado pelo Copilot para resolver a issue sobre exibir jogos relacionados na página de detalhes.

1. Volte ao repositório em GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações, volte à guia **Conversation** e role a página para baixo.
6. Você deverá ver um indicador de que alguns fluxos de trabalho aguardam aprovação.
7. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Os fluxos serão enfileirados na seção de verificações do PR. Se tudo correr bem, as verificações do aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode mudar para a branch do Codespace para testar manualmente a funcionalidade de jogos relacionados. No terminal do Codespace, execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot implementou a funcionalidade de jogos relacionados. Como antes, você pode iterar com o agente de nuvem para solicitar atualizações. Por exemplo, peça que ajuste a quantidade de jogos exibidos ou que adicione cabeçalhos de comentário e comentários TSDoc. Lembre-se de que essa issue foi atribuída **antes** da atualização das instruções personalizadas. Faça essas solicitações adicionando um comentário na guia **Conversation**; o Copilot iniciará uma nova sessão.

## Revisar os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados pelo agente personalizado. Este PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no Mission Control no Exercício 4.

1. Volte ao repositório em GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde alguns minutos até a conclusão.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, dando atenção especial a:
   - componentes de interface para alternar entre os modos.
   - uso do armazenamento local para persistir preferências.
   - alterações de CSS ou estilos para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que gerencia a alternância de modo.
6. Depois de revisar as atualizações, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os fluxos serão enfileirados na seção de verificações do PR. Se tudo correr bem, as verificações do aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** mude para essa branch no Codespace para testar manualmente os recursos de acessibilidade. No terminal do Codespace, execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Em seguida, inicie o aplicativo e teste os controles de alto contraste e modo claro no navegador. Confirme que funcionam e que as preferências persistem após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot para implementar essas funcionalidades seguindo boas práticas. Se notar problemas de acessibilidade ou oportunidades de melhoria, mencione **@copilot** em um comentário para solicitar alterações, como fez com os outros PRs.

## Exercício opcional — continuar delegando

O agente de nuvem funciona melhor quando você pode atribuir itens reais do backlog e cuidar de outras tarefas. Para desenvolver esse hábito, registre mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o roteiro do agente de nuvem. Ao longo dessas lições, você:

- **inspecionou os arquivos de instruções personalizados do repositório** para observar depois seu efeito nos resultados do agente de nuvem.
- **atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **criou e usou um agente personalizado** para acessibilidade, adicionando controles para modo de alto contraste e modo claro.
- **usou a página de agentes do Copilot como Mission Control** para acompanhar e orientar a sessão de acessibilidade durante a execução.
- **revisou e iterou sobre os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximas etapas

Você concluiu o roteiro do agente de nuvem. Para continuar explorando, os outros roteiros complementam o que você praticou:

- 🖥️ **[Roteiro do VS Code](../../vscode/)** — explore o modo Agent do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Roteiro da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal usando os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, Playwright MCP, Agent Merge e uma ponte opcional `/delegate` para o agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- atribua ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- crie um agente personalizado para outro domínio.
- configure `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
