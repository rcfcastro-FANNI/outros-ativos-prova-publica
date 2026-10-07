# Prova pública — WDO, BOVA11, ITAUSA e ZS1 (Roberta)

> ## ⏸️ Acompanhamento pausado desde 05/10/2026
>
> **O que aconteceu:** ao rodar o modelo ao vivo, notamos uma diferença
> entre os dados semanais históricos e os dados semanais ao vivo.
> Investigando, encontramos a causa: os dados semanais históricos usados
> nas análises e no ajuste do modelo incorporavam o fechamento da semana
> antes de a semana terminar — ou seja, "antecipavam" uma informação que,
> ao vivo, ainda não existe. O primeiro sintoma disso já tinha
> aparecido na correção da linha de
> 18/09/2026 no repositório do WIN (commit de 21/09/2026, que
> ajustou os valores semanais daquele dia) — na época tratado como um
> erro pontual de exportação, hoje sabemos que fazia parte do mesmo
> problema. Isso afeta todos os ativos acompanhados (WDO,
> BOVA11, ITAUSA e ZS1 aqui, e o WIN no repositório
> [win-prova-publica](https://github.com/rcfcastro-FANNI/win-prova-publica)).
>
> **O que isso significa:** o modelo atual (v1) foi ajustado sobre dados
> com esse problema, então os resultados históricos que embasaram a
> escolha dele não são confiáveis. Por isso decidimos pausar o
> acompanhamento, revisar o modelo e testá-lo de novo antes de continuar.
>
> **O que NÃO muda:** o histórico publicado até a última atualização
> (05/10/2026) fica preservado exatamente como está — nenhuma linha será
> editada ou apagada, e a corrente de hash continua verificável com
> `python verificar_integridade.py`.
>
> **Próximos passos:** previsão de retorno em cerca de um mês (início de
> novembro de 2026). O modelo revisado (v2) será publicado separado do
> v1, com uma corrente de hash nova e data de início própria, para que os
> dois períodos nunca se misturem.

Este repositório contém só o **histórico de resultados**, dia a dia, do
mesmo modelo de swing-trade (L/3, H4/H11) aplicado a 4 ativos em
acompanhamento paralelo ao WIN: WDO (mini dólar futuro), BOVA11 (ETF do
Ibovespa), ITAUSA (ação) e ZS1 (soja, futuro CBOT) — gerado
automaticamente, sem intervenção manual, desde que essa automação foi
criada pra cada um.

O histórico do WIN (o ativo principal, com o modelo original) fica num
repositório PRÓPRIO e separado: [win-prova-publica](https://github.com/rcfcastro-FANNI/win-prova-publica).
Este aqui é só pros outros 4, que entraram depois como acompanhamento
paralelo.

## O que tem aqui

- `registro_l3_wdo.csv`, `registro_l3_bova11.csv`, `registro_l3_itausa.csv`,
  `registro_l3_zs1.csv` — sinais de entrada/saída do modelo H4/H11 pra
  cada ativo, com o estado de uma posição hipotética dia a dia: abriu,
  manteve, fechou. Mesmo formato de coluna do `registro_l3.csv` do WIN —
  só o ativo de origem muda.
- `verificar_integridade.py` — um script simples, só com Python padrão
  (nenhuma biblioteca externa), que qualquer pessoa pode rodar pra
  conferir a integridade dos 4 históricos acima (ver a seção seguinte).

**O que NÃO tem aqui, de propósito:** nenhum código do modelo em si — a
lógica, os limiares, as hipóteses testadas. Isso é mantido privado. Este
repositório existe só pra tornar os *resultados* verificáveis
publicamente, não pra expor como o modelo funciona por dentro.

## Por que confiar nesse histórico

Cada linha nova, desde o primeiro dia de acompanhamento de cada ativo
(diferente do WIN, que ganhou essa proteção só depois de já ter
histórico — aqui a corrente cobre TUDO, sem período anterior
desprotegido), carrega um hash (SHA-256) calculado sobre os dados
daquele dia mais o hash da linha anterior — uma corrente. Se uma linha de
um dia já fechado fosse alterada depois (por exemplo, pra "corrigir" um
sinal que deu errado), essa corrente quebraria de um jeito detectável.

Rode, com Python instalado:

```
python verificar_integridade.py
```

Isso confere as 4 correntes (uma por ativo) e avisa exatamente qual linha
(se alguma) não bate mais com o que deveria. Além disso, o próprio
histórico de commits deste repositório no GitHub registra quando cada
atualização aconteceu — outra camada de verificação, independente do
hash.

## Colunas manuais

Nenhuma das colunas destes 4 arquivos é preenchida manualmente hoje (ao
contrário do `registro_l3.csv`/`registro_paper_trading.csv` do WIN, que
têm `resultado_real`/`observacoes`) — todas as colunas aqui são
automáticas, calculadas pelo modelo, e entram no hash.

## Status de cada ativo

Este acompanhamento está em período de validação — nenhum dos 4 tem
"relatório oficial" ainda, só o track record acumulando. Ver o painel
`status_paper_trading.md` (privado, não fica neste repositório) pra
situação do dia a dia de cada um.
