---
title: "AI Agentic Plugins, a infraestrutura que transforma prompts em ecossistema"
date: "2026-05-03"
draft: false
tags: ["ai", "agentes", "plugins", "mcp"]
categories: ["ai"]
description: "Como plugins agentic empacotam skills, subagentes, workflows e regras em componentes versionados — com ganhos de portabilidade e novos riscos de segurança."
---

> _"Plugins são para agentes o que extensões foram para o navegador e o que o npm foi para o JavaScript: a infraestrutura silenciosa que transforma uma ferramenta poderosa em um ecossistema."_

Plugins empacotam skills, subagentes, workflows e regras como componentes instaláveis e versionados — reduzindo o “mega-prompt”, aumentando portabilidade e exigindo mais cuidado com segurança.

A forma como interagimos com LLMs evoluiu em saltos curtos mas profundos: do chat livre para prompts estruturados em Markdown, destes para templates com variáveis, e agora para algo qualitativamente diferente — componentes com versão, escopo e distribuição. Se você ainda organiza seus prompts em pastas de texto solto, este artigo é sobre a infraestrutura que está substituindo isso.

A comunidade convergiu para um padrão aberto que transforma prompts e fluxos de trabalho em **componentes instaláveis, com controle de versão e portáveis**: os AI Agentic Plugins.


## Estrutura de um AI Agentic Plugin

Um plugin moderno não é um único arquivo de prompt. É um pacote coeso composto por engrenagens que se encaixam:

![AI Agent Plugin](/images/AI-Plugins.excalidraw.png)

### Skills — o "como fazer"

Skills são pacotes modulares de instruções que ensinam o agente a executar uma tarefa específica. Tipicamente, uma skill é uma pasta contendo um arquivo `SKILL.md` (com metadados YAML e instruções em Markdown), opcionalmente acompanhado por scripts, templates e documentos de referência.

A grande virada conceitual está no **progressive disclosure**: no início da sessão, o agente carrega apenas o nome e a descrição da skill. As instruções completas só entram em contexto quando a tarefa exige. Isso permite que um agente tenha _centenas_ de skills disponíveis sem inflar o consumo de tokens.

### Agents — o "quem faz"

Subagentes são personas especializadas com escopo, ferramentas e permissões próprios. Em vez de pedir a um único agente generalista que faça tudo, o plugin pode declarar agentes dedicados — um para revisão de segurança, outro para migração de banco de dados, outro para criar pull requests — cada um carregado apenas quando relevante.

### Workflow — o "quando e em que ordem"

Skills e agentes isolados não bastam. O que diferencia um plugin de uma simples coletânea de prompts é o **workflow**: hooks que disparam em pontos do ciclo de vida (antes de uma tool rodar, após uma escrita de arquivo, ao concluir uma tarefa), slash commands que iniciam fluxos compostos, e configurações MCP que conectam o agente a sistemas externos.

É o workflow que costura tudo: o agente certo é invocado no momento certo, com a skill certa, contra a fonte de dados certa.

### Instruções ou Regras — o "como sempre se comportar"

Regras são restrições globais que o agente respeita em toda interação dentro de um repositório. Diferente de skills, que são carregadas sob demanda, as regras entram em contexto automaticamente — o que exige disciplina na hora de escrevê-las.

Um arquivo de instruções do repositório na raiz do projeto (por exemplo, `CLAUDE.md`, `AGENTS.md` ou equivalente — depende da ferramenta) pode definir, por exemplo, que funções não devem ultrapassar 30 linhas, que commits devem seguir o padrão Conventional Commits, ou que determinados diretórios são somente leitura para o agente. Plugins podem empacotar e distribuir esses arquivos junto com suas skills e hooks, garantindo que toda a equipe opere com as mesmas restrições — sem depender de combinação verbal ou configuração manual.

Mantenha essas regras curtas e globais: cada byte de instrução permanente consome tokens em todas as iterações.

---

## Skill ≠ Plugin: a distinção que importa

Vale fixar uma diferença que confunde muita gente:

- **Skill** é um _padrão arquitetural_ — uma unidade autocontida de conhecimento.
- **Plugin** é um _mecanismo de distribuição_ — a embalagem versionada que entrega skills, hooks, agentes e configurações que executam tarefas para atingir um determinado objetivo.

Uma skill solta em `.claude/skills/` resolve um workflow pessoal. Um plugin é o que você publica no marketplace para que outras equipes consigam, com um único comando, ter exatamente o mesmo comportamento que você desenhou.

---

## Por que isso é uma mudança estrutural

A padronização traz quatro ganhos concretos:

**Modularidade real.** Skills são desenvolvidas, testadas e mantidas independentemente. Equipes especializam-se em domínios sem precisar entender o agente inteiro.

**Eficiência de contexto.** Carregar tudo numa única mega-prompt é o anti-padrão de 2024. [Pesquisas da Chroma](https://www.trychroma.com/research/context-rot) que testaram 18 modelos frontier mostram que todo modelo exibe degradação de performance à medida que o contexto cresce — fenômeno que a equipe chamou de *context rot*. No lado oposto, sistemas com carregamento modular colhem ganhos mensuráveis: o framework CodeAgents registrou [redução de 55–87% nos tokens de entrada](https://arxiv.org/abs/2507.03254) ao decompor tarefas entre agentes especializados com sub-rotinas reutilizáveis. Plugins entram nessa equação diretamente: o mecanismo de *progressive disclosure* — confirmado pela [documentação técnica da Anthropic](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) — garante que skills só entram no contexto quando a tarefa exige, e arquivos auxiliares consomem *zero tokens* enquanto não são necessários.

**Distribuição coerente.** Um workflow de "deploy serverless na AWS" pode envolver dezenas de regras, três subagentes e quatro conexões MCP. Como plugin, vira `/plugin install aws-serverless` e pronto.

**Portabilidade entre ferramentas.** O mesmo `SKILL.md` roda em agentes concorrentes — Claude Code, Cursor, Codex, Gemini CLI — desde que sigam o padrão aberto. A skill que você escreve hoje não morre se sua equipe migrar de ferramenta amanhã.

A combinação dos quatro ganhos tem um efeito multiplicador: um agente que aciona o subagente certo (modularidade), carrega apenas as skills necessárias (eficiência de contexto), usa um pacote versionado e auditável (distribuição) num formato portável (portabilidade) executa a mesma tarefa em menos iterações e com menos tokens do que um agente generalista carregado com todas as instruções de uma vez.

---

## O ecossistema atual

A linha do tempo é curta e densa. A tendência que se consolida em 2026 aponta para uma convergência rápida: plataformas como Anthropic, Vercel, OpenAI, Microsoft e Google caminham para adotar um padrão compartilhado de distribuição de skills e plugins — o equivalente ao npm para agentes. Até a data deste post, cada ferramenta ainda opera com seu próprio formato nativo, mas a pressão por interoperabilidade já é visível na forma como os padrões estão se aproximando.

### Ferramentas agentic que suportam plugins (o que “suportar” significa aqui)

Neste post, “suportar plugins” significa que existe algum mecanismo instalável/distribuível (loja, registro, repositório ou comando) que adiciona capacidades ao agente — como skills/commands, integrações e/ou workflows — mesmo que o formato exato varie entre plataformas.

A seguir, a lista de ferramentas que suportam plugins até a data deste post.

 1. **Claude Code** (Anthropic) 
 2. **Cursor**                  
 3. **OpenAI Codex CLI**        
 4. **Gemini CLI** (Google)     
 5. **GitHub Copilot**          


---

## Pontos de atenção ao instalar plugins e skills de terceiros

Padronização não é sinônimo de segurança. Plugins herdam as **permissões totais** do agente que estendem — acesso ao seu sistema de arquivos, chaves de API, credenciais, capacidade de executar comandos shell. Auditorias recentes encontraram problemas críticos (malware, prompt injection, segredos expostos) em uma fração não desprezível das skills publicadas em marketplaces abertos.

Boas práticas para times que vão adotar:

- **Prefira fontes oficiais** (Anthropic, fornecedores conhecidos, repositórios verificados).
- **Leia o `SKILL.md`** antes de instalar — instruções maliciosas podem se esconder em texto longo.
- **Limite escopo e permissões** quando o agente suportar.
- **Trate o catálogo como dependência**: pin de versão, atualização controlada, scanner de segurança no CI.

---

## Onde saber mais

Vou deixar aqui alguns links para quem quiser testar, colocar a mão na massa.

* [Loja oficial de plugins do Cursor](https://cursor.com/marketplace)
* [Loja de plugins do Gemini CLI](https://geminicli.com/extensions/)
* [Alguns plugins meus no github](https://github.com/ArquitetoMovel/markv-ai-plugins)
* [Plugins para Github Copilot CLI e VSCODE](https://github.com/github/awesome-copilot/blob/main/docs/README.plugins.md)
* [Plugins para Claude e Claude CODE](https://claude.com/plugins)

Importante destacar que um AI Agentic Plugin de uma determinada loja ou plataforma pode ser facilmente portável para outra.

### Pesquisa e referências técnicas

* [Context Rot – Chroma Research (18 modelos frontier)](https://www.trychroma.com/research/context-rot)
* [Equipping Agents for the Real World with Agent Skills – Anthropic Engineering](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)
* [Effective Context Engineering for AI Agents – Anthropic Engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
* [CodeAgents: Token-Efficient Multi-Agent Reasoning – ArXiv](https://arxiv.org/abs/2507.03254)
* [Stop Wasting Your Tokens: Efficient Runtime Multi-Agent Systems – ArXiv](https://arxiv.org/abs/2510.26585)
