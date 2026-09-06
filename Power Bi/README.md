# Modelo Semântico, Dicionário de Dados & Medidas DAX

Documentação técnica completa do modelo de dados utilizado no Dashboard de Logística: estrutura do modelo, dicionário de dados, medidas DAX e páginas do relatório.

## Sumário

- [Estrutura do Modelo (Esquema Estrela)](#️-estrutura-do-modelo-esquema-estrela)
- [Dicionário de Dados](#-dicionário-de-dados)
- [Medidas DAX](#-medidas-dax)
- [Páginas do Relatório](#️-páginas-do-relatório)
- [Sobre o formato PBIP](#-sobre-o-formato-pbip)

---

## Estrutura do Modelo (Esquema Estrela)

| Tabela | Tipo | Descrição |
|---|---|---|
| `logistica` | Fato | Um registro por pedido, com datas, status, cliente, motorista, valores e devolução |
| `dCalendario` | Dimensão | Calendário com Ano, Mês e Mês numérico, usado nas análises temporais e YoY |
| `Medida` | Tabela de medidas | Tabela "vazia" que centraliza todas as medidas DAX do modelo |
| `Parâmetro` / `Parâmetro 2` / `Param Fat, Ped e Devol` | Parâmetros | Parâmetros de campo, usados para alternar indicadores em visuais dinâmicos |

### Colunas da tabela fato `logistica`

`Nº Pedido` · `Itens` · `Data Emissão Pedido` · `Data Entrega Prevista` · `Destino` · `R$ Faturados` · `Saída para Entrega` · `Data Entrega Real` · `Qtd Devolução` · `Mot. Devolução` · `Dias Para Entrega` · `Status` · `Cliente` · `Motorista` · `Custo unit` · `Status devol`

### Relacionamentos

| De | Para | Observação |
|---|---|---|
| `logistica[Data Emissão Pedido]` | Tabela de datas local | Relação por data |
| `logistica[Data Entrega Prevista]` | Tabela de datas local | Relação por data |
| `logistica[Saída para Entrega]` | Tabela de datas local | Relação por data |
| `logistica[Data Entrega Real]` | `dCalendario[Date]` | Relação ativa usada nas análises temporais (YoY) |
| `dCalendario[Date]` | Tabela de datas local | Relação auxiliar |

> As "Tabelas de datas local" são geradas automaticamente pelo Power BI (hierarquia de datas) para cada campo de data usado nos visuais.

---

## Dicionário de Dados

### Tabela `logistica` (fato — 4.282 registros, um por pedido)

| Coluna | Tipo | Descrição | Domínio / Faixa observada |
|---|---|---|---|
| `Nº Pedido` | Texto | Identificador único do pedido | `A100` – `A999` |
| `Data Emissão Pedido` | Data | Data em que o pedido foi registrado no sistema | 27/01/2019 – 06/12/2021 |
| `Data Entrega Prevista` | Data | Data combinada com o cliente para a entrega | 06/02/2019 – 16/12/2021 |
| `Saída para Entrega` | Data | Data em que o pedido saiu do CD para entrega | 29/01/2019 – 19/12/2021 |
| `Data Entrega Real` | Data | Data em que o pedido de fato chegou ao cliente. Usada na relação ativa com `dCalendario` | 03/02/2019 – 20/12/2021 |
| `Destino` | Categórico | Cidade de destino do pedido | `BH`, `RJ`, `SP` |
| `Cliente` | Categórico | Rede varejista cliente (extraído de `Cliente-Motorista` na origem) | Walmart, Magazine Luiza, Americanas, Casas Bahia, Casa e Vídeo, Ricardo Eletro |
| `Motorista` | Categórico | Motorista responsável pela entrega (extraído de `Cliente-Motorista` na origem) | Felipe Silva, Túlio Silveira, Valdir Espinosa, Marcos Leroy, Luiz Pardal, João Gomes |
| `Itens` | Numérico (inteiro) | Quantidade de itens no pedido | 1 – 20 |
| `R$ Faturados` | Numérico (moeda) | Valor faturado do pedido | R$ 15,00 – R$ 1.020,00 |
| `Dias Para Entrega` | Numérico (dias) | Prazo prometido, em dias, para a entrega | 3 – 20 dias |
| `Status` | Categórico | Resultado da entrega frente ao prazo prometido | `No Prazo`, `Atrasado` |
| ` Qtd Devolução` | Numérico (inteiro) | Quantidade de itens devolvidos do pedido (0 quando não há devolução) | 0 – 14 |
| `Mot. Devolução` | Categórico | Motivo da devolução | `S/ Devolu.`, `Produto Errado`, `Danificado`, `Arrependimento` |
| `Status devol` | Categórico (derivada) | Indicador binário de devolução, calculado a partir de `Mot. Devolução` no Power Query | `Devolução`, `Sem devolução` |
| `Custo unit` | Numérico (moeda, derivada) | Custo unitário do item, calculado como `R$ Faturados / Itens` no Power Query | R$ 15,00 – R$ 51,00 |

> Nenhuma coluna possui valores nulos na base analisada (4.282 linhas, 0 nulos em todas as colunas).

### Tabela `dCalendario` (dimensão)

| Coluna | Tipo | Descrição |
|---|---|---|
| `Date` | Data | Data-chave da dimensão calendário, relacionada a `logistica[Data Entrega Real]` |
| `Ano` | Numérico (inteiro) | Ano extraído da data (2019, 2020, 2021) |
| `Mês` | Texto | Nome do mês, usado em rótulos de visuais |
| `NumMês` | Numérico (inteiro) | Número do mês (1–12), usado para ordenação correta do eixo |

### Cobertura temporal da base

| Ano | Pedidos | Observação |
|---|---|---|
| 2019 | 280 | Ano **parcial** — dados a partir de fevereiro |
| 2020 | 2.934 | Ano completo |
| 2021 | 1.068 | Ano **parcial** — dados até dezembro (incompleto) |

---

## Medidas DAX

Todas as medidas estão centralizadas na tabela `Medida`.

### Indicadores de pedidos e entregas

| Medida | Fórmula | Descrição |
|---|---|---|
| `Qtd de pedidos` | `COUNTROWS(logistica)` | Total de pedidos registrados |
| `No prazo` | `CALCULATE([Qtd de pedidos], logistica[Status]="No prazo")` | Pedidos entregues dentro do prazo |
| `Entregas atrasadas` | `CALCULATE(COUNTROWS(logistica), logistica[Status]="Atrasado")` | Pedidos entregues com atraso |
| `OTD` | `DIVIDE([No prazo],[Qtd de pedidos])` | *On Time Delivery* — % de pedidos no prazo |
| `Meta` | `0.9` | Meta de OTD (90%), usada como referência visual |
| `Lead Time Médio` | `AVERAGEX(logistica, DATEDIFF(Data Entrega Prevista, Data Entrega Real, DAY))` | Média de dias entre a data prevista e a real de entrega |
| `Produtos entregues completos` | `CALCULATE([No prazo], Mot. Devolução <> "S/ Devolu.")` | Pedidos no prazo e sem devolução |
| `Total de itens` | `SUM(logistica[Itens])` | Soma de itens despachados |

### Indicadores de devolução e faturamento

| Medida | Fórmula | Descrição |
|---|---|---|
| `Devoluções` | `CALCULATE(COUNTROWS(logistica), Mot. Devolução <> "s/ Devolu.")` | Quantidade de pedidos com devolução |
| `Taxa de devoluções` | `DIVIDE([Devoluções],[Qtd de pedidos])` | % de pedidos devolvidos |
| `Faturamento Total` | `SUM(logistica[R$ Faturados])` | Faturamento total do período |
| `Faturamento perdido` | `SUMX(logistica, [Qtd Devolução] * [Custo unit])` | Estimativa de receita perdida com devoluções |
| `% Faturamento perdido` | `[Faturamento perdido] / [Faturamento Total]` | % do faturamento perdido em devoluções |

### Comparativos ano a ano (YoY)

| Medida | Descrição |
|---|---|
| `Faturamento YoY` | Faturamento do mesmo período no ano anterior (`DATEADD(..., -1, YEAR)`) |
| `% Faturamento YTD` | Variação percentual do faturamento vs. ano anterior |
| `% pedidos` | Variação percentual da quantidade de pedidos vs. ano anterior |
| `% entregas atrasadas` | Variação percentual de entregas atrasadas vs. ano anterior |
| `% pedidos devolvidos` | Variação percentual de devoluções vs. ano anterior |
| `Texto Fat YOY` / `Texto % Pedidos YOY` / `Texto % Entregas YOY` / `Texto % Devol YOY` | Versões formatadas em texto (com ⇧/⇩) dos indicadores YoY acima, usadas em cartões de KPI |

### Medidas de suporte visual

| Medida | Descrição |
|---|---|
| `Cor Rótulos` | Retorna verde ou vermelho conforme a variação do faturamento YoY (formatação condicional) |
| `Coluna Valor Faturamento` | Formata o faturamento em Bi / Mi / K conforme a magnitude do valor |
| `100%` | Constante auxiliar (valor 1), usada como referência em visuais |
| `Dummy Fat` | Medida auxiliar para exibir o rótulo do % Faturamento YTD formatado com seta |

---

## Páginas do Relatório

| Página | Descrição |
|---|---|
| **Inicial** | Página de capa/abertura do dashboard |
| **Geral** | Visão consolidada: faturamento, pedidos, OTD, lead time e comparativos YoY |
| **Devoluções** | Detalhamento de devoluções: quantidade, motivos e faturamento perdido |

---

##  Sobre o formato PBIP

Este projeto foi salvo no formato **Power BI Project (.pbip)**, que separa o relatório e o modelo semântico em arquivos de texto (`.json` / `.tmdl`), permitindo:

- Versionamento com Git (diffs legíveis)
- Revisão de alterações em Pull Requests
- Separação entre camada de modelo (`*.SemanticModel`) e camada visual (`*.Report`)

Para editar, abra o arquivo `Dasboard-logistica-aula1.pbip` diretamente no Power BI Desktop.
