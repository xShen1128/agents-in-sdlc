---
title: "Exercício 5: Itere sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciamento de agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que adicionasse documentação ao código, criasse uma funcionalidade de jogos relacionados para a equipe de design iterar e implementasse recursos de acessibilidade, incluindo opções para alternar entre os modos de alto contraste e claro. Vamos explorar as mudanças no código e, se necessário, fornecer comentários ao Copilot para aprimorar o trabalho.

### Cenário

Como já foi destacado várias vezes, os fundamentos do design de software e do DevOps não mudam com a chegada da IA generativa. Sempre queremos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar a documentação, a funcionalidade de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, há algumas restrições de segurança para manter tudo seguro. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions, onde cria um ambiente separado e temporário para trabalhar.
- Todos os fluxos de trabalho do GitHub Actions precisam da aprovação de uma pessoa antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revise a documentação gerada

Vamos começar explorando o primeiro pull request (PR) gerado pelo agente de nuvem do GitHub Copilot: o PR que adiciona documentação ao código. Você fará isso usando a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, talvez você veja um aviso de que o GitHub Copilot está bloqueado por um firewall. Isso **é esperado**, porque o acesso do Copilot a recursos externos é limitado por padrão, incluindo chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request cujo título seja semelhante a **Add missing documentation** ou mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request incluirá o indicador **[WIP]**. Nesse caso, aguarde até que o Copilot conclua o trabalho. Isso pode levar alguns minutos; aproveite para fazer uma pausa ou refletir sobre o que aprendeu até agora.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os novos comentários TSDoc e outras documentações. As alterações exatas podem variar.

    Ao examinar as alterações, procure comentários TSDoc, convenções TypeScript e cabeçalhos de comentário. Eles vêm dos arquivos de instruções personalizadas revisados no início do Exercício 2.

6. Quando terminar a revisão e tudo parecer correto, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Você verá os fluxos de trabalho entrarem na fila da seção de verificações do pull request. Se tudo correr bem, as verificações do projeto passarão para o único aplicativo Astro. Isso pode levar alguns minutos.

## Solicite alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você também pode mencionar o Copilot em comentários — como faria com outros integrantes da equipe — no pull request ou em comentários associados a linhas de código. O Copilot verá os comentários e iniciará outra sessão para resolvê-los. Como os resultados não são determinísticos, não há um texto prescritivo para solicitar mudanças. Algumas ideias:

- Adicionar um breve cabeçalho de comentário no início de cada arquivo de código explicando sua finalidade.
- Adicionar comentários TSDoc aos arquivos TypeScript e Astro.
- Criar um README descrevendo a estrutura do aplicativo Astro.

1. Adicione um comentário pedindo uma alteração à documentação gerada e mencione **@copilot**, como faria com qualquer outra pessoa. Use uma das sugestões acima ou peça outra mudança na documentação do código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Para voltar ao pull request, selecione **Back to pull request**.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, você verá um novo commit no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Quando estiver tudo pronto, você poderá marcar o PR como pronto para revisão e mesclá-lo à branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise a funcionalidade de jogos relacionados

Vamos voltar ao PR criado pelo Copilot para resolver a issue sobre exibir jogos relacionados na página de detalhes dos jogos.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR cujo título seja semelhante a **Show related games on the game details page** ou mais descritivo.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Quando terminar a revisão e tudo parecer correto, volte à guia **Conversation** e role a página para baixo.
6. Você deverá ver um indicador de que alguns fluxos de trabalho aguardam aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Você verá os fluxos de trabalho entrarem na fila da seção de verificações do pull request. Se tudo correr bem, as verificações do projeto passarão para o único aplicativo Astro. Isso pode levar alguns minutos.
9. **Opcional:** você também pode alternar para essa branch no Codespace e testar manualmente a funcionalidade de jogos relacionados. Acesse o Codespace, abra o terminal e execute estes comandos (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot implementou a funcionalidade de jogos relacionados. Como antes, você pode iterar com o agente de nuvem do Copilot e solicitar atualizações. Por exemplo, você pode pedir ao Copilot que altere a quantidade de jogos relacionados exibidos ou que adicione cabeçalhos de comentário e comentários TSDoc. Lembre-se: essa tarefa foi atribuída **antes** da atualização das instruções personalizadas. Como antes, faça essas solicitações em um novo comentário na guia **Conversation**; o Copilot o verá e iniciará uma nova sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Este PR deve incluir tanto o modo de alto contraste solicitado no Exercício 3 quanto o modo claro solicitado no centro de controle no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR cujo título seja semelhante a **Add high contrast mode to website** ou mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request incluirá o indicador **[WIP]**. Nesse caso, aguarde até que ele conclua o trabalho. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - Componentes de interface para alternar entre os modos.
   - Uso do armazenamento local para salvar as preferências do usuário.
   - Alterações de CSS ou estilo dos modos de alto contraste e claro.
   - Atributos de acessibilidade, como rótulos ARIA e navegação pelo teclado.
   - Código JavaScript/TypeScript que gerencia a alternância entre os modos.

6. Quando terminar a revisão e tudo parecer correto, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Você verá os fluxos de trabalho entrarem na fila da seção de verificações do pull request. Se tudo correr bem, as verificações do projeto passarão para o único aplicativo Astro. Isso pode levar alguns minutos.
10. **Opcional:** alterne para essa branch no Codespace e teste manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute estes comandos (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme que eles funcionam e que as preferências permanecem após recarregar as páginas.

Observe como o agente personalizado de acessibilidade ajudou a orientar o Copilot para implementar os recursos seguindo boas práticas de acessibilidade. Se encontrar problemas ou melhorias possíveis, mencione **@copilot** em um comentário para pedir atualizações, como fez nos PRs anteriores.

## Exercício opcional — continue delegando

O agente de nuvem é mais útil quando você pode atribuir itens reais do backlog e se dedicar a outras atividades. Para desenvolver esse hábito, registre mais algumas issues no seu repositório e atribua-as ao Copilot. Algumas ideias:

- Criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- Implementar paginação na página com a lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizadas incluídos no repositório** para observar mais tarde seus efeitos no trabalho do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando opções para alternar entre os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como centro de controle** para acompanhar e orientar a sessão de acessibilidade em andamento.
- **Revisou e iterou sobre os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar mudanças e aprovando fluxos de trabalho.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Se quiser continuar explorando, os outros percursos complementam o que praticou aqui:

- 🖥️ **[Percurso de VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração MCP diretamente no IDE.
- 💻 **[Percurso de CLI](../../cli/)** — entregue duas mudanças revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, Playwright MCP, Agent Merge e uma integração opcional `/delegate` com o agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- Atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- Criar um agente personalizado para outra área.
- Configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o fluxo de trabalho de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciamento de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
