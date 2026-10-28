# Pipeline de Simulações Energéticas: PDF → PowerPoint

Automação em Python que monitora uma pasta, lê os PDFs de simulação energética que chegam nela, extrai os dados e devolve a apresentação pronta.

## O problema

As simulações energéticas chegavam como PDFs e precisavam ser transformadas, uma a uma, em apresentações para o cliente. A transcrição era manual: abrir o PDF, localizar cada valor, copiar para o template, conferir.

## A solução

Script Python que roda localmente e executa o ciclo completo:

1. **Monitoramento** — observa uma pasta sincronizada no OneDrive e detecta novos PDFs.
2. **Extração** — lê o documento com `pdfplumber` e localiza os valores por padrões de regex.
3. **Montagem** — preenche um template de PowerPoint via `python-pptx`, substituindo os marcadores pelos dados extraídos.
4. **Notificação** — dispara um e-mail avisando que a apresentação está pronta.

## Problemas técnicos resolvidos

- **Valores quebrados em múltiplas linhas do PDF** — o layout do documento parte alguns números entre linhas; foi preciso reconstruir o valor antes de interpretá-lo.
- **Marcadores divididos entre runs do PowerPoint** — o `python-pptx` fragmenta o texto de um parágrafo em runs independentes, então um marcador como `{{valor}}` pode estar partido entre três deles. A substituição precisou ser feita no nível do parágrafo, não do run.
- **Detecção dinâmica do ano** — o ano de referência varia por documento e é inferido do conteúdo, não fixado no código.

## Decisão técnica

A primeira alternativa avaliada foi Power Automate com Copilot Studio. Foi descartada por restrição de licenciamento — o Python local entrega o mesmo resultado sem custo adicional.

## Stack

`Python` · `pdfplumber` · `python-pptx` · `SMTP` · `OneDrive`

---

*Projeto construído no contexto de uma comercializadora de energia. O código e os dados não são públicos; este README documenta a arquitetura e os resultados.*
