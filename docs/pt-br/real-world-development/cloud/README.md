---
slug: pt-br/real-world-development/cloud
title: "Cloud agent do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O **[cloud agent do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que o GitHub Copilot trabalhe de forma assíncrona na nuvem. Você atribui tarefas no GitHub, e o cloud agent as executa em segundo plano — explora o repositório, faz alterações e abre um pull request (PR) — enquanto você fica livre para fazer outras coisas.

Neste conjunto de exercícios, você adicionará instruções personalizadas que o cloud agent seguirá e, depois, atribuirá uma issue do GitHub para que ele implemente o trabalho. Você revisará e usará agentes personalizados para orientar a abordagem, monitorará e orientará sessões no painel de agentes e concluirá revisando os pull requests e iterando sobre os resultados.

## Exercícios

| Exercício | Tópico | Descrição |
|----------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e seu codespace |
| [1. Instruções personalizadas][ex1] | Contexto | Adicione instruções personalizadas para o cloud agent seguir |
| [2. Cloud agent][ex2] | Agente assíncrono | Atribua issues ao cloud agent do Copilot |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revise e use agentes personalizados |
| [4. Gerenciar agentes][ex4] | Monitoramento | Monitore e oriente sessões de agentes |
| [5. Iterar][ex5] | Revisão | Revise PRs, itere sobre o trabalho do Copilot e escolha os próximos passos |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] O cloud agent do Copilot habilitado para sua conta ou organização

> [!TIP]
> Não tem um plano pago? Estudantes verificados podem obter o GitHub Copilot gratuitamente por meio do [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop — portanto, você pode concluir todos os percursos com ele.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> O cloud agent está disponível nos planos **Copilot Student, Pro, Pro+, Business e Enterprise** — todos, exceto o Copilot Free. No Copilot Business ou Enterprise, um administrador precisa habilitar o recurso para a organização.

> [!NOTE]
> Este percurso não aborda o Model Context Protocol (MCP). Para aprender a usar servidores MCP com o Copilot, consulte o [percurso da interface de linha de comando (CLI)](../cli/) ou o [percurso do VS Code](../vscode/).

## Primeiros passos

**[Comece pelo Exercício 0: Pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
