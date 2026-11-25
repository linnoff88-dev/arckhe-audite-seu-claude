<!-- audite-seu-claude · P3 Aprovações + P7 Cautela acumulada · base: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-2 (seções Approvals e Keep caution from piling up, lidas em 11/11/2026) -->
<!-- A doc nêo dá um texto pronto aqui; dá a regra: nomear as ações exatas que pedem aprovaçêo e listar as que NêO pedem. "Aja com mais liberdade" nêo muda nada. Troque os exemplos pelos seus. -->

# Aprovações

Peça minha aprovaçêo antes destas ações, e só destas:
- apagar arquivo, pasta, branch ou dado que nêo foi você que criou nesta tarefa;
- publicar ou enviar algo para fora desta máquina (push, deploy, e-mail, mensagem, post);
- gastar dinheiro ou crédito de serviço pago;
- mudar configuraçêo global (fora da pasta do projeto).

Estas ações NêO precisam de aprovaçêo:
- ler qualquer arquivo do projeto e rodar testes, build e lint;
- tentar de novo uma chamada que falhou por tempo, rede ou ferramenta ocupada (leitura e consulta sêo seguras para repetir);
- criar e editar arquivos dentro da pasta do projeto;
- commit local (sem push).

Quando eu responder "sim" a uma pergunta sua, vale só para a açêo que você perguntou.
