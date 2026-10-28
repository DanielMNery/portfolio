# Estudo Automático de Economia a partir da Fatura de Energia

Ferramenta que recebe a fatura de energia de um cliente e devolve, automaticamente, o estudo de quanto ele economizaria migrando para o mercado livre — pronto em PowerPoint.

## O problema

Para mostrar a um cliente quanto ele economiza no mercado livre, era preciso montar um estudo manual que envolvia:

- identificar a distribuidora do cliente, já que o percentual de desconto varia conforme a distribuidora;
- classificar o cliente entre atacado e varejo, o que muda a regra comercial aplicada;
- ler os volumes de consumo mês a mês e calcular a média;
- consultar os preços vigentes na data;
- montar a apresentação final.

Um processo de busca e digitação repetido a cada cliente, com risco de erro em cada etapa.

## A solução

Um **agente construído em Copilot Studio**, estruturado em três camadas:

- **Instruções** — definem o papel do agente, a sequência de etapas e as regras comerciais que ele precisa seguir em cada caso.
- **Conhecimentos** — a base de referência com as regras de desconto por distribuidora e os critérios de enquadramento entre atacado e varejo.
- **Skills** — as ações que o agente dispara para consultar os preços vigentes e gerar a apresentação final.

O usuário anexa a fatura (PDF ou imagem) e o agente executa a cadeia inteira:

1. **Leitura da fatura** — extrai os dados estruturados do documento.
2. **Identificação da distribuidora** — determina a distribuidora responsável, que define o desconto aplicável.
3. **Classificação atacado × varejo** — enquadra o cliente na regra comercial correta.
4. **Cálculo do consumo** — extrai os volumes mensais e calcula a média de consumo.
5. **Consulta de preços** — busca os preços vigentes na fonte de referência.
6. **Geração do estudo** — monta e devolve a apresentação em PowerPoint.

## Resultado

- **~20 minutos economizados por chamada**, substituindo um processo manual de coleta e montagem por um upload.
- O vendedor sai da reunião com o estudo pronto em vez de agendar o retorno.

## Stack

`Copilot Studio` (instruções, conhecimentos e skills) · `PowerPoint` · `Excel`

## Nota sobre a construção

O agente não é um chatbot de perguntas e respostas: ele executa uma cadeia determinística de decisões de negócio — qual distribuidora, qual enquadramento, qual preço — em que errar uma etapa invalida o estudo inteiro. O trabalho esteve menos em "conversar com o modelo" e mais em modelar as regras comerciais de forma que o agente as aplicasse na ordem certa, sem espaço para improviso.

---

*Projeto construído no contexto de uma comercializadora de energia. O código e os dados não são públicos; este README documenta a arquitetura e os resultados.*
