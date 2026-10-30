---
title: "Lição 8 - Criar e integrar o PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações de instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. É hora de revisá-los em conjunto e abrir um pull request. Você mesmo fez o merge do pull request (PR) de avaliações por estrelas; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações de instruções e da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, você manteve todo o fluxo de filtragem e qualidade em uma única sessão e branch, com todo esse trabalho incluído neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- examinar o PR completo do recurso e as evidências de verificação.
- autorizar o Agent Merge somente após a revisão e confirmar que o PR foi integrado.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização do merge sob o controle da pessoa desenvolvedora.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho restante para concluir um pull request no aplicativo GitHub Copilot. Quando você o habilita, a sessão do aplicativo lê o pull request e resolve o que estiver bloqueando o merge — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e faz o merge assim que o GitHub permite. Ele é executado em segundo plano, continua funcionando após reinicializações do aplicativo e é desativado automaticamente quando o pull request é mesclado.

Até aqui, você selecionou **Merge pull request** por conta própria. O Agent Merge pode assumir essa responsabilidade, mas a capacidade de editar código e mesclar ainda exige sua autorização explícita. Revise as ações permitidas e o trabalho antes de conceder permissão de merge.

## Usar o Agent Merge para gerenciar o PR

Com todo o código criado e revisado, vamos permitir que o Agent Merge gerencie o PR.

1. Use o seletor de agentes para escolher **Default agent**.
2. Selecione o menu suspenso ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Escolher a opção no menu suspenso, por si só, não inicia o processo.

O processo do Agent Merge será iniciado. Ele:

- criará o pull request com título e descrição.
- se a sessão tiver começado por uma issue, incluirá uma referência a ela no corpo da descrição.
- fará rebase ou resolverá possíveis conflitos com a branch de destino.
- acompanhará a CI para confirmar que todas as verificações foram aprovadas.
- monitorará o PR em busca de comentários de revisão de outras pessoas desenvolvedoras ou do Copilot e fará as atualizações necessárias para resolvê-los.
- poderá mesclar automaticamente o PR depois que tudo for aprovado.

Vamos permitir que o Agent Merge também mescle o PR depois que todas as verificações forem aprovadas.

5. Revise o diff do PR e as evidências de verificação antes de conceder permissão para mesclar.
6. Selecione o menu suspenso ao lado de **Agent Merge**.
7. Confirme que há uma marca ao lado de **Merge pull request**.
8. Peça explicitamente ao agente que faça a mesclagem:

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. Acompanhe o progresso e resolva os bloqueios informados. Confirme que o estado do PR é **Merged** antes de continuar; habilitar a permissão de mesclagem ou iniciar um agente não confirma que a mesclagem foi concluída.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem permissões ausentes. Resolva esses bloqueios antes de continuar.

## Resumo e próximos passos

Você automatizou várias partes do processo de desenvolvimento, incluindo a geração, o teste e a validação de código e, agora, o processo de pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- examinou o PR completo do recurso e as evidências de verificação.
- autorizou o Agent Merge somente após a revisão e confirmou que o PR foi integrado.

Em seguida, você [usará um canvas existente e criará um canvas de triagem][next-lesson] para explorar uma forma mais completa de examinar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app