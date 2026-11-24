# CLAUDE.md â€” audite-seu-claude

Kit que audita o Claude Code para o Fable 5.2: `varrer.py` (stdlib, sÃ³ leitura, sem rede), `blocos/` (textos oficiais traduzidos + aprovaÃ§Ãµes), `skill/`, guia em `guia/` (PT) + `guia/en/` + `guia/es/`.

## Regras do projeto
- SÃ³ leitura: o `varrer.py` nunca muda arquivo; sugestÃµes sÃ£o texto. Quem edita Ã© o Claude do usuÃ¡rio, com o `correcoes.md` aprovado.
- Regras e nÃºmeros vÃªm sÃ³ da pÃ¡gina oficial "Prompting Claude Fable 5.2" (lida em 11/11/2026). NÃ£o citar vÃ­deo, canal ou autor de terceiros.
- Regra nova = regex em `REGRAS` + caso em `tests/fixtures/sujo` + garantia de zero em `tests/fixtures/limpo`.
- Exemplos publicados: `--anonimo`, sem nomes de pastas privadas.
- TraduÃ§Ã£o (blocos, guia, README): `ARCKHÉ-traduz`.
- Autor dos commits: `linnoff88-dev <linnoff88-dev@gmail.com>`.
- Testes sem rede: `python3 -m unittest discover tests`.

## Self-learning

When I correct you, or you catch yourself making a mistake: before continuing, add the lesson as a one-line rule at the TOP of the Lessons list below, so it never happens again.

## Lessons
- O Ã­ndice de memÃ³ria que entra na sessÃ£o Ã© sÃ³ o do projeto aberto (`projects/<caminho-com-traÃ§os>/memory/MEMORY.md`); somar todos inflou o peso de 5 mil para 46 mil tokens (11/11/2026).
- Plugins: contar sÃ³ a versÃ£o mais nova em `plugins/cache/<loja>/<plugin>/`; `marketplaces/` e `temp_git_*` triplicavam os achados (11/11/2026).
- "Nunca" com inicial maiÃºscula: regex de palavra absoluta com `re.I` (11/11/2026).
