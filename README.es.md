ï»¿# audite-seu-claude

[![audite-seu-claude](guia/assets/banner-es.jpg)](https://linnoff88-dev.github.io/

ê°Å¸â¡Â§ê°Å¸â¡Â· [PortuguêÂªs](README.md) êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· **ê°Å¸â¡Âªê°Å¸â¡Â¸ EspaêÂ±ol**

## ê°Å¸ââ GuêÂ­a de uso

GuêÂ­a completa (landing + paso a paso): **https://linnoff88-dev.github.io/

## QuêÂ© es

Un auditor para tu Claude Code antes (o despuêÂ©s) de pasar a **Claude Fable 5.2**. Lee tu `CLAUDE.md`, skills, agentes, memorias, hooks, plugins y scripts, y encuentra las lêÂ­neas escritas para modelos anteriores que ahora estorban: la êÂ«herencia invisibleêÂ». Genera un informe con grêÂ¡ficos y una lista de sugerencias; marcas las que apruebas y tu propio Claude aplica solo esas.

Solo lectura: `varrer.py` no cambia ningêÂºn archivo. Sin red, solo Python 3 (biblioteca estêÂ¡ndar).

## Por quêÂ©

Anthropic publicêÂ³ la pêÂ¡gina [Prompting Claude Fable 5.2](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-2) con 10 puntos de atenciêÂ³n. Fable 5.2 sigue las instrucciones al pie de la letra, asêÂ­ que las lêÂ­neas antiguas cambian de efecto: pedir el razonamiento en el texto puede causar un rechazo, êÂ«responde sin pensarêÂ» no reduce el razonamiento, ordenar êÂ«usar siempre subagentesêÂ» multiplica el costo y las reglas acumuladas de êÂ«nuncaêÂ» frenan el trabajo rutinario. El orden recomendado por Anthropic: esfuerzo en `high`, limpiar las instrucciones antiguas, ejecutar las tareas largas y corregir un sêÂ­ntoma a la vez, midiendo.

## Inicio rêÂ¡pido

```bash
git clone https://github.com/linnoff88-dev/audite-seu-claude.git ~/audite-seu-claude
cd ~/meu-projeto
python3 ~/audite-seu-claude/varrer.py . --home --html relatorio.html --saida-json relatorio.json
# abre relatorio.html, marca lo que apruebas, haz clic en "gerar correcoes.md"
# en Claude Code: "aplique o correcoes.md"
```

## QuêÂ© busca

| Capa | Regla | Punto de Anthropic | Severidad |
|---|---|---|---|
| Fable 5.2 | Pide el razonamiento en el texto de la respuesta | P10 Rechazos | alta |
| Fable 5.2 | êÂ«Responde sin pensarêÂ» / êÂ«piensa menosêÂ» | P8 Instrucciones antiguas | media |
| Fable 5.2 | Presupuesto de tokens escrito en el prompt | P8 | media |
| Fable 5.2 | êÂ«Trata la respuesta anterior como cerradaêÂ» | P8 | media |
| Fable 5.2 | Regla rêÂ­gida de estilo (lêÂ­mite de palabras, telegrêÂ¡fico) | P8 | media |
| Fable 5.2 | Orden de delegar siempre a subagentes | P6 Subagentes | media |
| Fable 5.2 | Orden absoluta en mayêÂºsculas (êÂ«no es negociableêÂ») | P7 PrecauciêÂ³n acumulada | media |
| Fable 5.2 | êÂ«Nunca lo intentes de nuevoêÂ» / êÂ«detente ante el primer errorêÂ» | P7 | media |
| Fable 5.2 | AprobaciêÂ³n para êÂ«cualquier acciêÂ³nêÂ» | P3 Aprobaciones | media |
| Fable 5.2 | `claude -p "continue"` en un script | P3 | baja |
| Fable 5.2 | êÂ«NuncaêÂ» sin motivo ni alcance (una regla de seguridad se vuelve êÂ«protegidaêÂ») | P7 | baja / info |
| Fable 5.2 | êÂ«El cêÂ³digo es tuyoêÂ» | P4 Responsable del cêÂ³digo | info |
| Fable 5.2 | Esfuerzo `max` fijo, `xhigh` predeterminado, ajuste de modelo antiguo, Fable 5.2 en low/medium | P1 Esfuerzo | alta ê¢â â info |
| Fable 5.2 | Falta el texto para terminar la tarea / avisar del progreso / presentar un informe claro | P2, P5, P9 | info |
| Volumen | Peso fijo del contexto (CLAUDE.md, MEMORY.md, descripciones de skills) | ê¢â¬â | grêÂ¡fico |
| Volumen | Muchas skills, skills repetidas o parecidas, skills sin uso (`--uso 30`) | ê¢â¬â | baja / info |
| Volumen | Memorias sin cambios desde hace mêÂ¡s de 60 dêÂ­as; hooks y plugins activados | P7 | info |

## Contenido

| Archivo | QuêÂ© hace |
|---|---|
| `varrer.py` | El auditor. Opciones: `--home`, `--html`, `--saida-json`, `--json`, `--uso DIAS`, `--anonimo`, `--fundo N`, `--estrito` (CI), `--correcoes IDs --de relatorio.json`. |
| `blocos/` | Textos listos para pegar en `CLAUDE.md`: `terminar-tarefa.md`, `avisar-progresso.md`, `relatorio-claro.md` (traducciones de los textos oficiales, con el costo medido por Anthropic) y `aprovacoes.md` (modelo con la lista de acciones nombradas). Originales en `blocos/en/`. MêÂ¡s informaciêÂ³n en `biblioteca-skills.md`: cêÂ³mo guardar las skills que no usas todos los dêÂ­as. |
| `skill/audite-seu-claude/` | Skill para Claude Code: audita, espera tu aprobaciêÂ³n y aplica el `correcoes.md`. Copia a `~/.claude/skills/`. |
| `tests/` | 19 pruebas sin red: `python3 -m unittest discover tests`. |

## El proceso de principio a fin

| # | Paso | Comando | QuiêÂ©n |
|---|---|---|---|
| 1 | Descargar el kit | `git clone https://github.com/linnoff88-dev/audite-seu-claude.git ~/audite-seu-claude` | ê°Å¸Â§Â  têÂº |
| 2 | Poner Fable 5.2 en `high` para las tareas largas | `/effort high` (o `--effort high` en los robots) | ê°Å¸Â§Â  têÂº |
| 3 | Auditar | `python3 ~/audite-seu-claude/varrer.py . --home --html relatorio.html --saida-json relatorio.json` | ê¢Å¡â¢ê¯Â¸Â script |
| 4 | Leer el informe y marcar lo que apruebas | abrir `relatorio.html` ê¢â â êÂ«gerar correcoes.mdêÂ» y mover el archivo descargado a la carpeta del proyecto (o `varrer.py --correcoes A001,A004 --de relatorio.json --saida correcoes.md`) | ê°Å¸Â§Â  têÂº |
| 5 | Aplicar solo lo aprobado | en Claude Code: êÂ«aplique o correcoes.mdêÂ» | ê°Å¸Â¤â Claude |
| 6 | Revisar | ejecutar de nuevo el paso 3 y comparar | ê¢Å¡â¢ê¯Â¸Â script |
| 7 | Ejecutar la tarea mêÂ¡s larga y anotar dêÂ³nde se detiene, se queda callado o delega demasiado | ê¢â¬â | ê°Å¸Â§Â  têÂº |
| 8 | Para cada sêÂ­ntoma, pegar el bloque adecuado y medir (uno a la vez) | `blocos/*.md` | ê°Å¸Â§Â  + ê°Å¸Â¤â |

## Junto con el /doctor prompt-audit

Claude Code 2.1.283 o posterior tiene un auditor oficial: `/doctor prompt-audit`. Busca instrucciones escritas para modelos antiguos, archivos o comandos mencionados que ya no existen y reglas que se contradicen entre archivos, y propone las ediciones. Este kit cubre lo que el auditor no lee: memorias, textos pegados por plugins, prompts dentro de scripts y bots, el nivel de esfuerzo en `settings.json` y el peso de las skills, con un informe en grêÂ¡ficos. Ejecuta ambos.

Con Fable 5.2, Claude Code ya activa automêÂ¡ticamente el texto para terminar la tarea y el recordatorio cuando se queda callado (pruebas publicadas en la versiêÂ³n 2.1.322). Los bloques `terminar-tarefa.md` y `avisar-progresso.md` sirven para quienes llaman al modelo mediante la API o con un programa propio.

## Consejo: biblioteca de skills

Cada skill en `~/.claude/skills` carga su descripciêÂ³n en cada sesiêÂ³n. En vez de borrar las que usas poco, guêÂ¡rdalas en una **biblioteca**: una carpeta fuera de la carga (p. ej., `~/projetos/skills-reserva/.claude/skills`) con un `INDICE.md` (nombre, para quêÂ© sirve, palabras de bêÂºsqueda). Una lêÂ­nea en el `CLAUDE.md` global le indica a Claude que busque en el êÂ­ndice cuando pidas una skill que no estêÂ¡ cargada. Antes de moverla, comprueba si otra skill, script o timer la invoca. En la mêÂ¡quina del autor: 148 ê¢â â 107 skills cargadas, de 13,7 mil ê¢â â 9,6 mil tokens de descripciêÂ³n por sesiêÂ³n. Paso a paso y la lêÂ­nea lista: [`blocos/biblioteca-skills.md`](blocos/biblioteca-skills.md).

## Precauciones

- El auditor seêÂ±ala; têÂº decides. Las reglas de seguridad (API, crêÂ©dito, borrar, publicar) aparecen como **protegidas**: la sugerencia es definir su alcance y motivo, nunca borrarlas.
- Archivo de plugin: no lo edites (volverêÂ¡ con la actualizaciêÂ³n). Escribe en el `CLAUDE.md` global que esa regla no se aplica.
- Los textos de `blocos/` cuestan tokens (terminar: +~30%; avisar del progreso: +~20%, cifras de Anthropic). PêÂ©galos solo si observas el sêÂ­ntoma.

Licencia MIT.