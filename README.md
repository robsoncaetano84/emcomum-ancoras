# Âncoras do histórico do EmComum

Este repositório guarda, um arquivo por dia, a **raiz** do histórico do [EmComum](https://www.emcomumapp.com.br), app da RCTEC Soluções Tecnológicas para quem cuida de uma criança combinar a rotina.

## Para que serve

No EmComum nada se apaga: avisos, confirmações de leitura, trocas de dia e mudanças no calendário viram registros que não podem ser editados nem excluídos. Cada criança tem uma cadeia de registros em que cada um carrega o hash do anterior. Mexer em um registro antigo quebra a cadeia.

Todo dia, o banco resume a ponta de todas as cadeias num único hash (a raiz) e ela é publicada aqui, fora do banco. Depois de publicada, nem a própria empresa consegue reescrever o passado sem que a raiz recalculada deixe de bater com o arquivo deste repositório.

## O que tem em cada arquivo

`AAAA/MM/DD.txt`:

```
EmComum: âncora diária do histórico
dia: 2026-11-16 (até a meia-noite, horário de Brasília)
formato: v1
raiz: 3f1c...
```

Só o hash. Nenhum nome, número de crianças, conteúdo ou dado pessoal: não dá para descobrir nada a partir dele.

## Como a raiz é calculada (formato v1)

É uma árvore de Merkle: cada criança é uma folha. Assim, a família de uma criança consegue provar que a cadeia dela está na raiz do dia sem precisar ver nada das outras crianças (prova de inclusão).

```
folha  = SHA-256(0x00 || UTF-8(id_da_criança + "|" + número_do_último_registro + "|" + hash_do_último_registro_em_hex))
         uma por criança com registro antes da meia-noite (Brasília) do dia seguinte,
         em ordem do identificador da criança
nó     = SHA-256(0x01 || esquerda || direita); se sobrar um no fim do nível, ele sobe sem mudar
merkle = o nó que sobra no topo ("-" se não houver folha)
raiz   = SHA-256(UTF-8("emcomum|ancora|v1|" + AAAA-MM-DD + "|" + quantidade_de_folhas + "|" + merkle_em_hex))
```

Exemplo para conferir uma implementação: um dia sem nenhuma folha, `2000-01-01`, dá a raiz `f5ef27eec73774cad811033041d8befed539233adede36221f74cc46d727a164`.

Os arquivos são gravados por um processo automático e nunca são alterados depois de publicados. O histórico de commits deste repositório é público.

## Marco zero (17/11/2026)

Antes de o EmComum começar a ser usado de verdade, a base de testes foi zerada em 17/11/2026: saíram as crianças, os registros e as contas de teste. A âncora de 16/11/2026 se refere a esses dados de teste e não pode mais ser recalculada. As âncoras de uso real começam em 17/11/2026.
