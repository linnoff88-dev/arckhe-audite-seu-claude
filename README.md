ï»¿# audite-seu-claude

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![audite-seu-claude](guia/assets/banner.jpg)](https://linnoff88-dev.github.io/

**ê°Å¸â¡Â§ê°Å¸â¡Â· PortuguêÂªs êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

## ê°Å¸ââ Guia de uso

Guia completo (landing + passo a passo): **https://linnoff88-dev.github.io/

## O que êÂ©

Um auditor para o seu Claude Code antes (ou depois) de passar para o **Claude Fable 5.2**. Ele lêÂª o seu `CLAUDE.md`, skills, agentes, memêÂ³rias, hooks, plugins e scripts, e acha as linhas escritas para modelos anteriores que agora atrapalham ê¢â¬â a "heranêÂ§a invisêÂ­vel". Gera um relatêÂ³rio com grêÂ¡ficos e uma lista de sugestêÂµes; vocêÂª marca o que aprova, e o seu prêÂ³prio Claude aplica sêÂ³ isso.

SêÂ³ leitura: o `varrer.py` nêÂ£o muda nenhum arquivo. Sem rede, sêÂ³ Python 3 (biblioteca padrêÂ£o).

## Por quêÂª

A Anthropic publicou a pêÂ¡gina [Prompting Claude Fable 5.2](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-2) com 10 pontos de atenêÂ§êÂ£o. O Fable 5.2 segue instruêÂ§êÂµes ao pêÂ© da letra, entêÂ£o linhas antigas mudam de efeito: pedir o raciocêÂ­nio no texto pode dar recusa, "responda sem pensar" nêÂ£o reduz o pensamento, ordem de "sempre usar subagentes" multiplica o custo, regras "nunca" acumuladas seguram trabalho de rotina. A ordem que a Anthropic recomenda: esforêÂ§o em `high`, limpar as instruêÂ§êÂµes antigas, rodar as tarefas longas, corrigir um sintoma por vez medindo.

## ComeêÂ§o rêÂ¡pido

```bash
git clone https://github.com/linnoff88-dev/audite-seu-claude.git ~/audite-seu-claude
cd ~/meu-projeto
python3 ~/audite-seu-claude/varrer.py . --home --html relatorio.html --saida-json relatorio.json
# abra relatorio.html, marque o que aprova, clique em "gerar correcoes.md"
# no Claude Code: "aplique o correcoes.md"
```

## O que ele procura

| Camada | Regra | Ponto da Anthropic | Severidade |
|---|---|---|---|
| Fable 5.2 | Pede o raciocêÂ­nio no texto da resposta | P10 Recusas | alta |
| Fable 5.2 | "Responda sem pensar" / "pense menos" | P8 InstruêÂ§êÂµes antigas | mêÂ©dia |
| Fable 5.2 | OrêÂ§amento de tokens escrito no prompt | P8 | mêÂ©dia |
| Fable 5.2 | "Trate a resposta anterior como encerrada" | P8 | mêÂ©dia |
| Fable 5.2 | Regra rêÂ­gida de estilo (limite de palavras, telegrêÂ¡fico) | P8 | mêÂ©dia |
| Fable 5.2 | Ordem de sempre delegar a subagentes | P6 Subagentes | mêÂ©dia |
| Fable 5.2 | Ordem absoluta em caixa alta ("nêÂ£o êÂ© negociêÂ¡vel") | P7 Cautela acumulada | mêÂ©dia |
| Fable 5.2 | "Nunca tente de novo" / "pare no primeiro erro" | P7 | mêÂ©dia |
| Fable 5.2 | AprovaêÂ§êÂ£o para "qualquer aêÂ§êÂ£o" | P3 AprovaêÂ§êÂµes | mêÂ©dia |
| Fable 5.2 | `claude -p "continue"` em script | P3 | baixa |
| Fable 5.2 | "Nunca" sem motivo nem alcance (regra de seguranêÂ§a vira "protegida") | P7 | baixa / info |
| Fable 5.2 | "O cêÂ³digo êÂ© seu" | P4 Dono do cêÂ³digo | info |
| Fable 5.2 | EsforêÂ§o `max` fixo, `xhigh` padrêÂ£o, ajuste de modelo antigo, Fable 5.2 em low/medium | P1 EsforêÂ§o | alta ê¢â â info |
| Fable 5.2 | Falta o texto de terminar a tarefa / avisar progresso / relatêÂ³rio claro | P2, P5, P9 | info |
| Volume | Peso do contexto fixo (CLAUDE.md, MEMORY.md, descriêÂ§êÂµes de skills) | ê¢â¬â | grêÂ¡fico |
| Volume | Muitas skills, skill repetida ou parecida, skill sem uso (`--uso 30`) | ê¢â¬â | baixa / info |
| Volume | MemêÂ³rias sem mudanêÂ§a hêÂ¡ mais de 60 dias; hooks e plugins ligados | P7 | info |

## ConteêÂºdo

| Arquivo | O que faz |
|---|---|
| `varrer.py` | O auditor. OpêÂ§êÂµes: `--home`, `--html`, `--saida-json`, `--json`, `--uso DIAS`, `--anonimo`, `--fundo N`, `--estrito` (CI), `--correcoes IDs --de relatorio.json`. |
| `blocos/` | Textos prontos para colar no `CLAUDE.md`: `terminar-tarefa.md`, `avisar-progresso.md`, `relatorio-claro.md` (traduêÂ§êÂµes dos textos oficiais, com o custo medido pela Anthropic) e `aprovacoes.md` (modelo com a lista de aêÂ§êÂµes nomeadas). Originais em `blocos/en/`. Mais `biblioteca-skills.md`: como guardar as skills que vocêÂª nêÂ£o usa todo dia. |
| `skill/audite-seu-claude/` | Skill para o Claude Code: audita, espera a sua aprovaêÂ§êÂ£o, aplica o `correcoes.md`. Copie para `~/.claude/skills/`. |
| `tests/` | 19 testes sem rede: `python3 -m unittest discover tests`. |

## O processo, do inêÂ­cio ao fim

| # | Passo | Comando | Quem |
|---|---|---|---|
| 1 | Baixar o kit | `git clone https://github.com/linnoff88-dev/audite-seu-claude.git ~/audite-seu-claude` | ê°Å¸Â§Â  vocêÂª |
| 2 | PêÂ´r o Fable 5.2 em `high` nas tarefas longas | `/effort high` (ou `--effort high` nos robêÂ´s) | ê°Å¸Â§Â  vocêÂª |
| 3 | Auditar | `python3 ~/audite-seu-claude/varrer.py . --home --html relatorio.html --saida-json relatorio.json` | ê¢Å¡â¢ê¯Â¸Â script |
| 4 | Ler o relatêÂ³rio e marcar o que aprova | abrir `relatorio.html` ê¢â â "gerar correcoes.md" e mover o arquivo baixado para a pasta do projeto (ou `varrer.py --correcoes A001,A004 --de relatorio.json --saida correcoes.md`) | ê°Å¸Â§Â  vocêÂª |
| 5 | Aplicar sêÂ³ o aprovado | no Claude Code: "aplique o correcoes.md" | ê°Å¸Â¤â Claude |
| 6 | Conferir | rodar o passo 3 de novo e comparar | ê¢Å¡â¢ê¯Â¸Â script |
| 7 | Rodar a tarefa mais longa e anotar onde ele para, cala ou delega demais | ê¢â¬â | ê°Å¸Â§Â  vocêÂª |
| 8 | Para cada sintoma, colar o bloco certo e medir (um por vez) | `blocos/*.md` | ê°Å¸Â§Â  + ê°Å¸Â¤â |

## Junto com o /doctor prompt-audit

O Claude Code 2.1.283 ou mais novo tem um auditor oficial: `/doctor prompt-audit`. Ele procura instruêÂ§êÂµes escritas para modelos antigos, arquivos ou comandos citados que nêÂ£o existem mais e regras que se contradizem entre arquivos, e propêÂµe as ediêÂ§êÂµes. Este kit cobre o que ele nêÂ£o lêÂª: memêÂ³rias, textos colados por plugins, prompts dentro de scripts e bots, o nêÂ­vel de esforêÂ§o no `settings.json` e o peso das skills, com relatêÂ³rio em grêÂ¡ficos. Rode os dois.

Com o Fable 5.2, o Claude Code jêÂ¡ liga sozinho o texto de terminar a tarefa e o lembrete quando ele fica calado (testes publicados na versêÂ£o 2.1.322). Os blocos `terminar-tarefa.md` e `avisar-progresso.md` servem para quem chama o modelo pela API ou por programa prêÂ³prio.

## Dica: biblioteca de skills

Toda skill em `~/.claude/skills` carrega a descriêÂ§êÂ£o em toda sessêÂ£o. Em vez de apagar as que vocêÂª usa pouco, guarde-as numa **biblioteca**: uma pasta fora do carregamento (ex.: `~/projetos/skills-reserva/.claude/skills`) com um `INDICE.md` (nome, para quêÂª, palavras de busca). Uma linha no `CLAUDE.md` global manda o Claude buscar no êÂ­ndice quando vocêÂª pedir uma skill que nêÂ£o estêÂ¡ carregada. Antes de mover, confira se outra skill, script ou timer chama a skill. Na mêÂ¡quina do autor: 148 ê¢â â 107 skills carregadas, 13,7 mil ê¢â â 9,6 mil tokens de descriêÂ§êÂ£o por sessêÂ£o. Passo a passo e a linha pronta: [`blocos/biblioteca-skills.md`](blocos/biblioteca-skills.md).

## Cuidados

- O auditor sinaliza; quem decide êÂ© vocêÂª. Regra de seguranêÂ§a (API, crêÂ©dito, apagar, publicar) aparece como **protegida**: a sugestêÂ£o êÂ© dar alcance e motivo, nunca apagar.
- Arquivo de plugin: nêÂ£o edite (volta na atualizaêÂ§êÂ£o). Escreva no `CLAUDE.md` global que aquela regra nêÂ£o vale.
- Os textos de `blocos/` custam tokens (terminar: +~30%; avisar progresso: +~20%, nêÂºmeros da Anthropic). Cole sêÂ³ se vocêÂª vêÂª o sintoma.

LicenêÂ§a MIT.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>