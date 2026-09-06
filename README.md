#  Dashboard de Logística

Projeto de análise de dados logísticos utilizando **Power BI** e **DAX**, com foco em desempenho de entregas, devoluções e faturamento.

![Capa do Dashboard](Imagens/capa_dashboard_logistica.png)

---

##  Visão Geral

O projeto foi desenvolvido de ponta a ponta em Power BI, contemplando:

- ✅ Modelagem de dados (esquema estrela)
- ✅ Tratamento e organização de colunas na camada Power Query
- ✅ Criação de medidas DAX (indicadores de entrega, devolução e faturamento)
- ✅ Análise comparativa Ano vs Ano (YoY)
- ✅ Dashboard interativo com 3 páginas
- ✅ Storytelling executivo dos resultados

---

##  Objetivo do Projeto

Monitorar a operação logística de ponta a ponta — desde a emissão do pedido até a entrega ao cliente — identificando gargalos de prazo, motivos de devolução e o impacto financeiro dessas ocorrências no faturamento.

---

##  Perguntas de Negócio

> Os pedidos estão sendo entregues dentro do prazo combinado?
> Qual o volume e o custo das devoluções para a operação?
> Como o faturamento e o volume de pedidos evoluem em relação ao ano anterior?

---

## Arquitetura do Projeto

```
Base de dados (pedidos de logística)
        │
        ▼
   Power Query (tratamento e tipagem)
        │
        ▼
Modelo Semântico (esquema estrela: fato logistica + dim dCalendario)
        │
        ▼
   Medidas DAX (tabela "Medida")
        │
        ▼
   Dashboard Power BI (3 páginas)
```

---

## Principais Indicadores (KPIs)

| Indicador | Resultado |
|---|---|
| Pedidos analisados | 4.282 |
| Faturamento total | R$ 1.451.761,70 |
| **OTD** (entregas no prazo) | **26,8%** (meta: 90%) |
| Entregas atrasadas | 73,2% |
| **Taxa de devoluções** | **74,1%** |
| Faturamento perdido em devoluções | R$ 206.296,90 (14,2% do faturamento) |

> A lista completa das medidas DAX, com as fórmulas, está documentada em [`Power Bi/README.md`](Power Bi/README.md). A leitura executiva completa dos números está em [`Business Analytics/README.md`](Business Analytics/README.md).

---

## Diagnóstico Executivo

A operação está muito abaixo da meta de OTD (26,8% vs. 90%), com um padrão de falha **uniforme entre destinos, motoristas e clientes** — ou seja, o problema é estrutural, não pontual. Nas devoluções, **66,7%** vêm de erro operacional ("Produto Errado" + "Danificado"), não de arrependimento do cliente (7,5%), apontando para uma falha no processo de separação/conferência dos pedidos.

---

## Dashboard Power BI

O relatório possui 3 páginas:

| Página | Conteúdo |
|---|---|
| **Inicial** | Capa / abertura do dashboard |
| **Geral** | Visão consolidada de faturamento, pedidos, OTD e comparativos YoY |
| **Devoluções** | Análise de devoluções, motivos e faturamento perdido |

> 💡 Adicione aqui os prints das páginas do dashboard (pasta `Imagens/`) para ilustrar os resultados, por exemplo:
> `![Página Geral](Imagens/dashboard_geral.png)`

---

## Estrutura do Repositório

```
Dashboard-Logistica
│── README.md
│
├── Imagens/
│   └── capa_dashboard_logistica.png
│
├── Power_BI/
│   ├── README.md
│   ├── Dasboard-logistica-aula1.pbip
│   ├── Dasboard-logistica-aula1.Report/
│   └── Dasboard-logistica-aula1.SemanticModel/
│
└── Business_Analytics/
    └── README.md
```

---

## Documentações Técnicas

- [Power BI, Modelo Semântico e Medidas DAX](Power_BI/README.md)
- [Apresentação Executiva](Business_Analytics/README.md)

---

## Competências Demonstradas

✅ Power BI  
✅ Modelagem de Dados (Esquema Estrela)  
✅ DAX (medidas simples, YoY, formatação condicional)  
✅ Power Query  
✅ Análise de Indicadores Logísticos (OTD, Lead Time, Devoluções)  
✅ Storytelling com Dados  
✅ Versionamento de projetos Power BI com Git (PBIP)

---

## Como abrir o projeto

Este projeto está no formato **PBIP** (Power BI Project), que permite versionamento em Git.

1. Instale o [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (versão com suporte a PBIP habilitado).
2. Clone este repositório.
3. Abra o arquivo `Power_BI/Dasboard-logistica-aula1.pbip` no Power BI Desktop.

---

## Autora: Viviane de Andrade

Projeto desenvolvido para fins de estudo e portfólio em Análise de Dados / Business Intelligence.
