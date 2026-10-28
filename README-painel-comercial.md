# Painel Comercial — migração de Power BI para aplicação web

Migração do dashboard comercial de Power BI para uma aplicação que roda no navegador, construída em HTML, CSS, JavaScript, Python e Flask, com fidelidade validada número a número.

> Projeto **sem IA no produto** — a aplicação é determinística, não há modelo em nenhuma etapa. Entra neste portfólio por outro motivo: é a prova de que consigo levar uma ferramenta de dado até o deploy. A IA entrou como **ferramenta de construção**, acelerando a engenharia reversa do modelo original e a escrita do código.

## O problema

O dashboard comercial da empresa — que eu mesmo havia desenvolvido em Power BI e que virou a principal ferramenta de consulta de todos os colaboradores — dependia de licenciamento e do ambiente Microsoft para cada acesso. A proposta foi reconstruí-lo como aplicação web própria, mantendo o mesmo visual e, principalmente, os mesmos números.

## A solução

Aplicação web autônoma — front-end em HTML, CSS e JavaScript, back-end em Python com Flask — com autenticação própria, consumindo as mesmas bases que alimentavam o BI:

- **Página Carteira** — KPIs de unidades consumidoras, MWm, preço médio por fonte, com seis gráficos e tabela detalhada.
- **Página Propostas** — funil de propostas (totais, únicas, fechadas), taxa de conversão, motivos de perda, cortes por submercado e por gestora.
- **Filtro cruzado** — clicar em qualquer visual destaca a seleção e filtra todos os outros, replicando o comportamento do Power BI (cada visual não se auto-filtra pela própria dimensão).
- **Exportação para Excel** — gera `.xlsx` real respeitando os filtros ativos, com colunas extras para reconciliação.

## O trabalho de verdade: reconciliação

Replicar o visual é rápido. Fazer os números baterem é que foi o projeto.

O modelo original envolvia transformações em Power Query (M), dezenas de medidas DAX e colunas calculadas, muitas delas com regras não documentadas. Para reproduzir com fidelidade:

- Construí um **extrator automático do arquivo `.pbit`** que varre os visuais, as medidas DAX, as colunas calculadas e os relacionamentos do modelo, gerando um mapa navegável em HTML — o gabarito para reconstruir cada página sem adivinhar.
- Reproduzi a lógica real linha a linha: regra de composição da carteira, expansão de contratos por ano de fornecimento, deduplicação de unidades consumidoras por operação, tratamento de registros órfãos.
- Rastreei e expliquei **cada divergência** entre o painel e o BI até a diferença cair para **1 unidade consumidora em cerca de 2.500** — e essa última foi comprovadamente um registro ausente na amostra de dados, não um erro de lógica.

Entre os achados: um padrão de código de operação que o split ingênuo por separador interpretava errado quando o sufixo era o nome de um local, resolvido com regex; e uma flag de deduplicação aplicada onde o BI não a aplicava, que estava descartando os maiores contratos da base.

## Stack

`HTML` · `CSS` · `JavaScript` · `Highcharts` · `Python` · `Flask` · `pandas` · `openpyxl` · `pytest`

## Qualidade

Suíte de testes automatizados cobrindo as regras de negócio reconstruídas, travando cada fórmula validada contra o modelo original.

## Onde a IA entrou

Não no produto — no processo. Usei IA como par de construção para acelerar a engenharia reversa das medidas DAX e do Power Query do modelo original, para escrever o código do front-end e do back-end e para investigar as divergências numéricas até cada uma ter uma causa identificada. A validação final de cada número foi feita contra o BI de produção.

---

*Projeto construído no contexto de uma comercializadora de energia. O código e os dados não são públicos; este README documenta a arquitetura e os resultados.*
