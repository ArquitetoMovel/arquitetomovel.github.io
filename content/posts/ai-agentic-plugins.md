+++
date = '2026-05-03T18:22:52-03:00'
draft = true
title = 'Ai Agentic Plugins'
+++

# AI Agentic Plugins - Padronizando e distribuindo prompts reutilizáveis

> _"Plugins são para agentes o que extensões foram para o navegador e o que o npm foi para o JavaScript: a infraestrutura silenciosa que transforma uma ferramenta poderosa em um ecossistema."_

A forma na qual interagimos com uma *LLM* vem mudando drasticamente nos últimos anos,  começamos fazendo perguntas no chat , passando por prompts mais estruturados com formatação _markdown_ até chegar em especializações como _skills_, _subagents_ , _comandos_ etc.

Em 2026, esse cenário mudou. A comunidade convergiu para um padrão aberto que transforma prompts e fluxos de trabalho em **componentes instaláveis, com controle de versão e portável**: os AI Agent Plugins.

---

## A anatomia de um plugin

Um plugin moderno não é um único arquivo de prompt. É um pacote coeso composto por engrenagens que se encaixam:

![[AI Plugins.excalidraw.png]]
### Skills — o "como fazer"

Skills são pacotes modulares de instruções que ensinam o agente a executar uma tarefa específica. Tipicamente, uma skill é uma pasta contendo um arquivo `SKILL.md` (com metadados YAML e instruções em Markdown), opcionalmente acompanhado por scripts, templates e documentos de referência.

A grande virada conceitual está no **progressive disclosure**: no início da sessão, o agente carrega apenas o nome e a descrição da skill. As instruções completas só entram em contexto quando a tarefa exige. Isso permite que um agente tenha _centenas_ de skills disponíveis sem inflar o consumo de tokens.

###  Agents — o "quem faz"

Subagentes são personas especializadas com escopo, ferramentas e permissões próprios. Em vez de pedir a um único agente generalista que faça tudo, o plugin pode declarar agentes dedicados — um para revisão de segurança, outro para migração de banco de dados, outro para criar pull requests — cada um carregado apenas quando relevante.

###  Workflow — o "quando e em que ordem"

Skills e agentes isolados não bastam. O que diferencia um plugin de uma simples coletânea de prompts é o **workflow**: hooks que disparam em pontos do ciclo de vida (antes de uma tool rodar, após uma escrita de arquivo, ao concluir uma tarefa), slash commands que iniciam fluxos compostos, e configurações MCP que conectam o agente a sistemas externos.

É o workflow que costura tudo: o agente certo é invocado no momento certo, com a skill certa, contra a fonte de dados certa.

### Instruções ou Regras - "boas praticas para serem seguidas sempre"

Regras que são seguidas no escopo do repositório através de arquivos _AGENTS.md_, ou instruções que são aplicadas por um determinado tipo de arquivo. 
Por exemplo, no processo de escrita de código não queremos que as funções ultrapasse 30 linhas etc.
Esses tipos de arquivos precisam ter regras concisas e globais, pois são carregados em todas iterações. 

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

**Eficiência de contexto.** Carregar tudo numa única mega-prompt é o anti-padrão de 2024. Plugins com carregamento condicional só pagam o custo em tokens daquilo que a tarefa atual exige.

**Distribuição coerente.** Um workflow de "deploy serverless na AWS" pode envolver dezenas de regras, três subagentes e quatro conexões MCP. Como plugin, vira `/plugin install aws-serverless` e pronto.

**Portabilidade entre ferramentas.** O mesmo `SKILL.md` roda em agentes concorrentes — Claude Code, Cursor, Codex, Gemini CLI — desde que sigam o padrão aberto. A skill que você escreve hoje não morre se sua equipe migrar de ferramenta amanhã.

---

## O ecossistema atual

A linha do tempo é curta e densa. A Anthropic lançou o conceito de Agent Skills em outubro de 2025 e o publicou como padrão aberto em dezembro do mesmo ano. Em janeiro de 2026, a Vercel disponibilizou o **skills.sh**, descrito por muitos como "o npm dos agentes". OpenAI, Microsoft e Google aderiram em sequência.

### Ferramentas agentic que  suportam AI Plugin

Abaixo segue a lista de ferramentas que suportam plugin até a data desse post.

| #   | Ferramenta                  |
| --- | --------------------------- |
| 1   | **Claude Code** (Anthropic) |
| 2   | **Cursor**                  |
| 3   | **OpenAI Codex CLI**        |
| 4   | **Gemini CLI** (Google)     |
| 5   | **GitHub Copilot**          |


---

## Pontos de atenção ao instalar plugins e SKILLS de terceiros

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
