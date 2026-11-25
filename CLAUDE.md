# CLAUDE.md ê¢â¬â audite-seu-claude

Kit que audita o Claude Code para o Fable 5.2: `varrer.py` (stdlib, sêÂ³ leitura, sem rede), `blocos/` (textos oficiais traduzidos + aprovaêÂ§êÂµes), `skill/`, guia em `guia/` (PT) + `guia/en/` + `guia/es/`.

## Regras do projeto
- SêÂ³ leitura: o `varrer.py` nunca muda arquivo; sugestêÂµes sêÂ£o texto. Quem edita êÂ© o Claude do usuêÂ¡rio, com o `correcoes.md` aprovado.
- Regras e nêÂºmeros vêÂªm sêÂ³ da pêÂ¡gina oficial "Prompting Claude Fable 5.2" (lida em 11/11/2026). NêÂ£o citar vêÂ­deo, canal ou autor de terceiros.
- Regra nova = regex em `REGRAS` + caso em `tests/fixtures/sujo` + garantia de zero em `tests/fixtures/limpo`.
- Exemplos publicados: `--anonimo`, sem nomes de pastas privadas.
- TraduêÂ§êÂ£o (blocos, guia, README): `ARCKHê-traduz`.
- Autor dos commits: `linnoff88-dev <linnoff88-dev@gmail.com>`.
- Testes sem rede: `python3 -m unittest discover tests`.

## Self-learning

When I correct you, or you catch yourself making a mistake: before continuing, add the lesson as a one-line rule at the TOP of the Lessons list below, so it never happens again.

## Lessons
- O êÂ­ndice de memêÂ³ria que entra na sessêÂ£o êÂ© sêÂ³ o do projeto aberto (`projects/<caminho-com-traêÂ§os>/memory/MEMORY.md`); somar todos inflou o peso de 5 mil para 46 mil tokens (11/11/2026).
- Plugins: contar sêÂ³ a versêÂ£o mais nova em `plugins/cache/<loja>/<plugin>/`; `marketplaces/` e `temp_git_*` triplicavam os achados (11/11/2026).
- "Nunca" com inicial maiêÂºscula: regex de palavra absoluta com `re.I` (11/11/2026).
