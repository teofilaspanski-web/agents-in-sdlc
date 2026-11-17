---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas voltadas à melhoria da experiência do usuário e à adição de funcionalidades. Você pediu ao Copilot que documentasse o código, criasse uma funcionalidade de jogos relacionados para a equipe de design explorar e implementasse recursos de acessibilidade, incluindo opções de alto contraste e modo claro. Vamos examinar as alterações e, se necessário, enviar feedback ao Copilot para aprimorar o trabalho.

### Cenário

Como já destacamos várias vezes, os fundamentos do design de software e do DevOps não mudam com a inclusão da IA generativa. Sempre precisamos revisar o código gerado e seguir nosso processo normal de DevOps. Vamos revisar as sugestões do GitHub Copilot para criar documentação, a funcionalidade de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão pelo restante da equipe.

## Segurança e cloud agent do GitHub Copilot

Como o cloud agent do Copilot executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para garantir que tudo continue protegido. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à ramificação que usará para o código.
- O cloud agent é executado no GitHub Actions, que cria um ambiente separado e temporário para o trabalho.
- Todos os workflows do GitHub Actions precisam da aprovação de uma pessoa antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revisar a documentação gerada

Vamos começar explorando o primeiro pull request (PR) gerado pelo cloud agent do GitHub Copilot: o PR que adiciona documentação ao código. Você fará isso na interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você talvez veja um aviso informando que o firewall bloqueou o GitHub Copilot. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, incluindo chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do cloud agent do Copilot][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título semelhante a **Add missing documentation** ou outro título mais completo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos, então você pode fazer uma pausa ou refletir sobre o que aprendeu até agora.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui novos comentários TSDoc e outros elementos de documentação. As alterações exatas variam.

    Ao revisar as alterações, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Eles são definidos nos arquivos de instruções personalizadas examinados no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a tela para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o único app Astro serão aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você também pode mencioná-lo em comentários — como faria com qualquer integrante da equipe — no pull request ou em comentários no código. O Copilot verá esses comentários e iniciará outra sessão para atender à solicitação. Como os resultados são não determinísticos, não podemos fornecer um texto exato para o pedido. Algumas ideias de alterações que você pode solicitar:

- adicionar cabeçalhos de comentário no início de cada arquivo de código, com uma breve descrição do que ele faz.
- adicionar comentários TSDoc a arquivos TypeScript e Astro.
- criar um README com uma descrição da estrutura do app Astro.

1. Adicione um comentário solicitando uma alteração à documentação gerada e mencione **@copilot**, como faria com qualquer usuário. Use uma das ideias acima ou outra sugestão de documentação que você queira ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Depois que o Copilot concluir as alterações, deverá aparecer um novo commit no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você poderá marcar o PR como pronto para revisão e mesclá-lo à ramificação principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar a funcionalidade de jogos relacionados

Vamos voltar ao PR que o Copilot gerou para resolver a issue de exibir jogos relacionados na página de detalhes de cada jogo.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page** ou outro título mais completo.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a tela para baixo.
6. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o único app Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você também pode selecionar essa ramificação no Codespace para testar manualmente a funcionalidade de jogos relacionados. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da ramificação criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou a funcionalidade de jogos relacionados! Como antes, você pode iterar com o cloud agent do Copilot e solicitar atualizações. Por exemplo, peça ao Copilot que altere a quantidade de jogos relacionados exibidos ou adicione cabeçalhos de comentário e comentários TSDoc. (Lembre-se: essa tarefa foi atribuída **antes** da atualização das instruções personalizadas!) Como nos PRs anteriores, adicione um novo comentário na guia **Conversation**. O Copilot verá o comentário e iniciará uma nova sessão.

## Revisar os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Esse PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel de agentes do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website** ou outro título mais completo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação e preste atenção especial a:
   - componentes da interface para alternar entre os modos.
   - uso do armazenamento local para persistir as preferências do usuário.
   - alterações de CSS ou estilo dos modos de alto contraste e claro.
   - atributos de acessibilidade (rótulos do padrão Accessible Rich Internet Applications (ARIA), navegação por teclado etc.).
   - código JavaScript/TypeScript que gerencia a alternância de modos.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a tela para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o único app Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** você pode selecionar essa ramificação no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da ramificação criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o app e teste as opções de alto contraste e modo claro no navegador para confirmar se funcionam como esperado e persistem após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot na implementação desses recursos de acordo com as práticas recomendadas de acessibilidade. Se encontrar problemas ou possíveis melhorias de acessibilidade, mencione **@copilot** em um comentário e peça as alterações, como nos PRs anteriores.

## Exercício opcional — continuar delegando

O cloud agent funciona melhor quando você pode atribuir a ele itens reais do backlog e se concentrar em outras tarefas. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na página de lista de jogos.
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do cloud agent. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizadas deste repositório** para observar mais tarde o efeito deles no trabalho do cloud agent.
- **Atribuiu issues ao cloud agent do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando opções de alto contraste e modo claro.
- **Usou a página de agentes do Copilot como painel de controle** para monitorar e orientar a sessão de acessibilidade em andamento.
- **Revisou e iterou sobre pull requests do cloud agent**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do cloud agent. Para continuar explorando, os outros percursos complementam o que você praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, MCP do Playwright, Agent Merge e uma ponte opcional `/delegate` para voltar ao cloud agent.

No seu repositório, experimente estas ideias:

- atribuir ao cloud agent uma issue de refatoração ou de cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do cloud agent do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do cloud agent do Copilot][agent-firewall]

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
