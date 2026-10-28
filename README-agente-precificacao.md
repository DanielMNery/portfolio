# Agente de Precificação de Propostas de Energia

Automação que recebe pedidos de cotação por e-mail, calcula o preço do produto e devolve o resultado no Teams — eliminando a etapa mais lenta da elaboração de uma proposta comercial.

## O problema

No mercado livre de energia, cada proposta comercial leva cerca de **15 minutos** para ser montada. Desses, de **7 a 10 minutos** são só precificação, porque o processo exige:

- buscar informações espalhadas em planilhas Excel diferentes;
- aplicar cálculos distintos dependendo do produto contratado;
- aplicar cálculos distintos dependendo do período de fornecimento;
- interpretar e-mails que chegam de **mais de 126 gestoras parceiras**, cada uma com seu próprio formato.

Com um volume de 30 a 100 propostas por dia (pico mensal de 800), essa etapa virou o gargalo do time.

## A solução

Fluxo ponta a ponta construído em **Power Automate**:

1. **Gatilho** — monitora a caixa de entrada e identifica e-mails de pedido de cotação.
2. **Extração** — interpreta o corpo do e-mail e separa os parâmetros da cotação (produto, volume, período, submercado).
3. **Precificação** — busca os preços vigentes nas planilhas de referência e aplica a regra de cálculo correspondente ao produto e ao período.
4. **Entrega** — publica o resultado em um canal do Teams, onde o time comercial consome direto.

## Decisão técnica: agente de IA vs. script

A primeira versão usava um **agente em Copilot Studio** para interpretar o e-mail e distinguir o produto. Funcionava, mas o **custo por execução** inviabilizou a operação no volume real.

Substituí o componente por um **Office Script com regex**, mantendo o mesmo resultado e zerando o custo recorrente da etapa. O agente foi avaliado, medido e trocado quando os números não fecharam — não descartado por preconceito nem mantido por entusiasmo.

## Resultado

- **~10 minutos economizados por execução** (estimativa medida pelo próprio Power Automate).
- **~5 horas economizadas por semana** no volume atual de execuções.
- Em produção, usado pelo time comercial.

## Stack

`Power Automate` · `Office Script (regex)` · `Excel` · `Microsoft Teams` · `Copilot Studio` (versão inicial)

## Limitação conhecida

Cada gestora envia o e-mail em um formato próprio, e qualquer desvio de formatação quebra a extração. O caminho em avaliação é um fallback por modelo de linguagem para os casos que o regex não cobre, acionado só na exceção para manter o custo controlado.

---

*Projeto construído no contexto de uma comercializadora de energia. O código e os dados não são públicos; este README documenta a arquitetura e os resultados.*
