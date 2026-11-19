# audite-seu-claude

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![audite-seu-claude](guia/assets/banner.jpg)](https://inematds.github.io/audite-seu-claude/guia/)

**ðŸ‡§ðŸ‡· PortuguÃªs Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

## ðŸ“– Guia de uso

Guia completo (landing + passo a passo): **https://inematds.github.io/audite-seu-claude/guia/**

## O que Ã©

Um auditor para o seu Claude Code antes (ou depois) de passar para o **Claude Fable 5.2**. Ele lÃª o seu `CLAUDE.md`, skills, agentes, memÃ³rias, hooks, plugins e scripts, e acha as linhas escritas para modelos anteriores que agora atrapalham â€” a "heranÃ§a invisÃ­vel". Gera um relatÃ³rio com grÃ¡ficos e uma lista de sugestÃµes; vocÃª marca o que aprova, e o seu prÃ³prio Claude aplica sÃ³ isso.

SÃ³ leitura: o `varrer.py` nÃ£o muda nenhum arquivo. Sem rede, sÃ³ Python 3 (biblioteca padrÃ£o).

## Por quÃª

A Anthropic publicou a pÃ¡gina [Prompting Claude Fable 5.2](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-2) com 10 pontos de atenÃ§Ã£o. O Fable 5.2 segue instruÃ§Ãµes ao pÃ© da letra, entÃ£o linhas antigas mudam de efeito: pedir o raciocÃ­nio no texto pode dar recusa, "responda sem pensar" nÃ£o reduz o pensamento, ordem de "sempre usar subagentes" multiplica o custo, regras "nunca" acumuladas seguram trabalho de rotina. A ordem que a Anthropic recomenda: esforÃ§o em `high`, limpar as instruÃ§Ãµes antigas, rodar as tarefas longas, corrigir um sintoma por vez medindo.

## ComeÃ§o rÃ¡pido

```bash
git clone https://github.com/inematds/audite-seu-claude.git ~/audite-seu-claude
cd ~/meu-projeto
python3 ~/audite-seu-claude/varrer.py . --home --html relatorio.html --saida-json relatorio.json
# abra relatorio.html, marque o que aprova, clique em "gerar correcoes.md"
# no Claude Code: "aplique o correcoes.md"
```

## O que ele procura

| Camada | Regra | Ponto da Anthropic | Severidade |
|---|---|---|---|
| Fable 5.2 | Pede o raciocÃ­nio no texto da resposta | P10 Recusas | alta |
| Fable 5.2 | "Responda sem pensar" / "pense menos" | P8 InstruÃ§Ãµes antigas | mÃ©dia |
| Fable 5.2 | OrÃ§amento de tokens escrito no prompt | P8 | mÃ©dia |
| Fable 5.2 | "Trate a resposta anterior como encerrada" | P8 | mÃ©dia |
| Fable 5.2 | Regra rÃ­gida de estilo (limite de palavras, telegrÃ¡fico) | P8 | mÃ©dia |
| Fable 5.2 | Ordem de sempre delegar a subagentes | P6 Subagentes | mÃ©dia |
| Fable 5.2 | Ordem absoluta em caixa alta ("nÃ£o Ã© negociÃ¡vel") | P7 Cautela acumulada | mÃ©dia |
| Fable 5.2 | "Nunca tente de novo" / "pare no primeiro erro" | P7 | mÃ©dia |
| Fable 5.2 | AprovaÃ§Ã£o para "qualquer aÃ§Ã£o" | P3 AprovaÃ§Ãµes | mÃ©dia |
| Fable 5.2 | `claude -p "continue"` em script | P3 | baixa |
| Fable 5.2 | "Nunca" sem motivo nem alcance (regra de seguranÃ§a vira "protegida") | P7 | baixa / info |
| Fable 5.2 | "O cÃ³digo Ã© seu" | P4 Dono do cÃ³digo | info |
| Fable 5.2 | EsforÃ§o `max` fixo, `xhigh` padrÃ£o, ajuste de modelo antigo, Fable 5.2 em low/medium | P1 EsforÃ§o | alta â†’ info |
| Fable 5.2 | Falta o texto de terminar a tarefa / avisar progresso / relatÃ³rio claro | P2, P5, P9 | info |
| Volume | Peso do contexto fixo (CLAUDE.md, MEMORY.md, descriÃ§Ãµes de skills) | â€” | grÃ¡fico |
| Volume | Muitas skills, skill repetida ou parecida, skill sem uso (`--uso 30`) | â€” | baixa / info |
| Volume | MemÃ³rias sem mudanÃ§a hÃ¡ mais de 60 dias; hooks e plugins ligados | P7 | info |

## ConteÃºdo

| Arquivo | O que faz |
|---|---|
| `varrer.py` | O auditor. OpÃ§Ãµes: `--home`, `--html`, `--saida-json`, `--json`, `--uso DIAS`, `--anonimo`, `--fundo N`, `--estrito` (CI), `--correcoes IDs --de relatorio.json`. |
| `blocos/` | Textos prontos para colar no `CLAUDE.md`: `terminar-tarefa.md`, `avisar-progresso.md`, `relatorio-claro.md` (traduÃ§Ãµes dos textos oficiais, com o custo medido pela Anthropic) e `aprovacoes.md` (modelo com a lista de aÃ§Ãµes nomeadas). Originais em `blocos/en/`. Mais `biblioteca-skills.md`: como guardar as skills que vocÃª nÃ£o usa todo dia. |
| `skill/audite-seu-claude/` | Skill para o Claude Code: audita, espera a sua aprovaÃ§Ã£o, aplica o `correcoes.md`. Copie para `~/.claude/skills/`. |
| `tests/` | 19 testes sem rede: `python3 -m unittest discover tests`. |

## O processo, do inÃ­cio ao fim

| # | Passo | Comando | Quem |
|---|---|---|---|
| 1 | Baixar o kit | `git clone https://github.com/inematds/audite-seu-claude.git ~/audite-seu-claude` | ðŸ§  vocÃª |
| 2 | PÃ´r o Fable 5.2 em `high` nas tarefas longas | `/effort high` (ou `--effort high` nos robÃ´s) | ðŸ§  vocÃª |
| 3 | Auditar | `python3 ~/audite-seu-claude/varrer.py . --home --html relatorio.html --saida-json relatorio.json` | âš™ï¸ script |
| 4 | Ler o relatÃ³rio e marcar o que aprova | abrir `relatorio.html` â†’ "gerar correcoes.md" e mover o arquivo baixado para a pasta do projeto (ou `varrer.py --correcoes A001,A004 --de relatorio.json --saida correcoes.md`) | ðŸ§  vocÃª |
| 5 | Aplicar sÃ³ o aprovado | no Claude Code: "aplique o correcoes.md" | ðŸ¤– Claude |
| 6 | Conferir | rodar o passo 3 de novo e comparar | âš™ï¸ script |
| 7 | Rodar a tarefa mais longa e anotar onde ele para, cala ou delega demais | â€” | ðŸ§  vocÃª |
| 8 | Para cada sintoma, colar o bloco certo e medir (um por vez) | `blocos/*.md` | ðŸ§  + ðŸ¤– |

## Junto com o /doctor prompt-audit

O Claude Code 2.1.283 ou mais novo tem um auditor oficial: `/doctor prompt-audit`. Ele procura instruÃ§Ãµes escritas para modelos antigos, arquivos ou comandos citados que nÃ£o existem mais e regras que se contradizem entre arquivos, e propÃµe as ediÃ§Ãµes. Este kit cobre o que ele nÃ£o lÃª: memÃ³rias, textos colados por plugins, prompts dentro de scripts e bots, o nÃ­vel de esforÃ§o no `settings.json` e o peso das skills, com relatÃ³rio em grÃ¡ficos. Rode os dois.

Com o Fable 5.2, o Claude Code jÃ¡ liga sozinho o texto de terminar a tarefa e o lembrete quando ele fica calado (testes publicados na versÃ£o 2.1.322). Os blocos `terminar-tarefa.md` e `avisar-progresso.md` servem para quem chama o modelo pela API ou por programa prÃ³prio.

## Dica: biblioteca de skills

Toda skill em `~/.claude/skills` carrega a descriÃ§Ã£o em toda sessÃ£o. Em vez de apagar as que vocÃª usa pouco, guarde-as numa **biblioteca**: uma pasta fora do carregamento (ex.: `~/projetos/skills-reserva/.claude/skills`) com um `INDICE.md` (nome, para quÃª, palavras de busca). Uma linha no `CLAUDE.md` global manda o Claude buscar no Ã­ndice quando vocÃª pedir uma skill que nÃ£o estÃ¡ carregada. Antes de mover, confira se outra skill, script ou timer chama a skill. Na mÃ¡quina do autor: 148 â†’ 107 skills carregadas, 13,7 mil â†’ 9,6 mil tokens de descriÃ§Ã£o por sessÃ£o. Passo a passo e a linha pronta: [`blocos/biblioteca-skills.md`](blocos/biblioteca-skills.md).

## Cuidados

- O auditor sinaliza; quem decide Ã© vocÃª. Regra de seguranÃ§a (API, crÃ©dito, apagar, publicar) aparece como **protegida**: a sugestÃ£o Ã© dar alcance e motivo, nunca apagar.
- Arquivo de plugin: nÃ£o edite (volta na atualizaÃ§Ã£o). Escreva no `CLAUDE.md` global que aquela regra nÃ£o vale.
- Os textos de `blocos/` custam tokens (terminar: +~30%; avisar progresso: +~20%, nÃºmeros da Anthropic). Cole sÃ³ se vocÃª vÃª o sintoma.

LicenÃ§a MIT.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>