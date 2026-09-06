# Apresentação Executiva — Dashboard de Logística

> Dados extraídos e calculados diretamente da base `BD_Logistica.xlsx` (4.282 pedidos, período de 03/02/2019 a 20/12/2021), replicando as mesmas transformações do Power Query do modelo.

---

##  Sumário Executivo

A operação apresenta desempenho crítico: **quase 3 em cada 4 pedidos chegam atrasados** e **quase 3 em cada 4 pedidos têm algum tipo de devolução** — muito abaixo da meta de 90% de entregas no prazo (OTD) definida no modelo.

| Indicador | Resultado |
|---|---|
| Pedidos analisados | 4.282 |
| Faturamento total | R$ 1.451.761,70 |
| Itens despachados | 44.931 |
| **OTD (entregas no prazo)** | **26,8%** (meta: 90% → gap de **63,2 p.p.**) |
| Entregas atrasadas | 3.135 (73,2%) |
| **Taxa de devoluções** | **74,1%** |
| Faturamento perdido em devoluções | R$ 206.296,90 (**14,2%** do faturamento total) |
| Atraso médio (nos pedidos atrasados) | 4,9 dias (mediana: 5 dias) |

**O que os dados revelam:**

1. **A operação está estruturalmente fora da meta** — apenas 26,8% dos pedidos são entregues no prazo, contra uma meta de 90%. Não é um desvio pontual, é o padrão da operação.
2. **O problema é sistêmico, não pontual** — atraso e devolução se distribuem de forma quase idêntica entre os 3 destinos, os 6 motoristas e os 6 clientes da base (todos entre ~71% e ~79%). Não existe um "vilão" isolado — o gargalo está no processo, não em uma pessoa, rota ou cliente específico.
3. **Erro operacional, não arrependimento do cliente, domina as devoluções** — "Produto Errado" (35,9%) e "Danificado" (30,8%) juntos respondem por quase 2 em cada 3 devoluções, contra apenas 7,5% por "Arrependimento".

---

## O Desafio de Negócio

A operação logística cresceu ao longo de 2019–2021 (de 280 pedidos em 2019, ainda parcial, para 2.934 em 2020), mas o crescimento em volume não veio acompanhado de controle de qualidade: o percentual de atraso e de devolução se manteve consistentemente alto em todos os períodos e segmentos analisados.

> A pergunta que motiva esta análise: **por que quase 3 em cada 4 entregas falham (no prazo ou na integridade do produto), independentemente de motorista, destino ou cliente?**

---

##  Capítulo 1 — O Tamanho do Problema

- **4.282** pedidos no período (fev/2019 a dez/2021)
- **R$ 1.451.761,70** em faturamento total
- **44.931** itens despachados
- **3.135** pedidos atrasados (**73,2%**) vs. **1.147** no prazo (**26,8%**)

**Leitura de negócio:** a meta definida no modelo é de 90% de OTD (*On Time Delivery*). O resultado atual está **63,2 pontos percentuais abaixo da meta** — isso não é uma operação com ajustes finos a fazer, é uma operação que precisa de intervenção estrutural.

---

##  Capítulo 2 — Os Padrões

### Padrão 1 — O atraso é a regra, não a exceção

| Métrica | Valor |
|---|---|
| Pedidos atrasados | 3.135 (73,2%) |
| Atraso médio (entre os atrasados) | 4,9 dias |
| Atraso mediano | 5 dias |
| Atraso máximo observado | 10 dias |

> Entre os pedidos entregues no prazo, a maioria chega até 1–2 dias *antes* do previsto — ou seja, quando a operação funciona, ela funciona bem. O problema não é a meta ser inatingível: é a inconsistência do processo.

### Padrão 2 — Atraso e devolução são uniformes entre destino, motorista e cliente

| Destino | Pedidos | Taxa de atraso |
|---|---|---|
| BH | 1.496 | 73,3% |
| SP | 1.412 | 73,2% |
| RJ | 1.374 | 73,1% |

| Motorista (top volume) | Pedidos | Taxa de atraso |
|---|---|---|
| Luiz Pardal | 750 | 76,0% |
| Marcos Leroy | 753 | 75,3% |
| Valdir Espinosa | 732 | 73,0% |
| João Gomes | 685 | 72,1% |
| Túlio Silveira | 698 | 71,8% |
| Felipe Silva | 664 | 70,6% |

> A variação entre o melhor e o pior motorista é de apenas ~5 p.p., e entre destinos é praticamente nula. Isso descarta hipóteses de "motorista ruim" ou "rota problemática" como causa principal — o gargalo está em algo comum a toda a operação (ex.: prazo prometido irreal, capacidade do CD, ou processo de despacho).

### Padrão 3 — Devolução concentrada em falha operacional, não em arrependimento

| Motivo da devolução | Pedidos | % do total | Faturamento perdido |
|---|---|---|---|
| Produto Errado | 1.537 | 35,9% | R$ 101.067,90 |
| Danificado | 1.317 | 30,8% | R$ 86.310,90 |
| Arrependimento | 321 | 7,5% | R$ 18.918,10 |
| Sem devolução | 1.107 | 25,9% | — |

> **66,7%** das devoluções vêm de erro de separação/manuseio (produto errado + danificado), contra apenas 7,5% de arrependimento do cliente. O problema está predominantemente **dentro da operação**, não na decisão do cliente.

---

##  Impacto Financeiro

- **R$ 206.296,90** perdidos em devoluções — equivalente a **14,2%** de todo o faturamento do período.
- "Produto Errado" sozinho já responde por quase metade dessa perda (R$ 101.067,90).

---

##  Diagnóstico Executivo

A operação de logística opera hoje muito abaixo da meta de OTD (26,8% vs. 90%), com um padrão de falha **uniforme e sistêmico**: atraso e devolução ocorrem em proporções semelhantes independentemente de destino, motorista ou cliente. Isso indica que a causa raiz não está em um elo fraco isolado, mas em um problema estrutural do processo — muito provavelmente relacionado a prazos de entrega mal dimensionados e falhas no processo de separação/conferência de pedidos, já que "Produto Errado" e "Danificado" concentram dois terços das devoluções.

---

##  Recomendações de Negócio

| Ação | Tipo | Descrição |
|---|---|---|
| Auditar o processo de separação e conferência (picking) | **Imediata** | "Produto Errado" e "Danificado" somam 66,7% das devoluções e R$ 187 mil em perdas — é o ponto de maior retorno potencial |
| Revisar o SLA de entrega prometido | **Estrutural** | Com 73,2% dos pedidos atrasados de forma uniforme entre motoristas e destinos, o prazo prometido hoje provavelmente não reflete a capacidade real da operação |
| Investigar a causa raiz do atraso sistêmico | **Investigativa** | Como o padrão é uniforme, a causa provavelmente está no despacho/CD (ex.: gargalo de saída), não nas rotas individuais — vale mapear o processo ponta a ponta |

---

##  Riscos e Limitações da Análise

1. **2019 e 2021 são anos parciais** na base (2019 começa em fevereiro; 2021 termina em dezembro incompleto) — comparações diretas de faturamento ano a ano devem ser lidas com cautela.
2. **Sem dados de custo logístico** (frete, combustível, mão de obra) — o "Faturamento perdido" mede apenas o valor dos itens devolvidos, não o custo total do retrabalho.
3. **A causa raiz exata dos erros de picking não está nos dados** — a análise aponta *onde* o problema está concentrado, mas a investigação de *por que* (treinamento, processo, sistema) requer levantamento qualitativo com a operação.

---

##  Próximos Passos

- Mapear o processo de separação de pedidos para identificar o ponto de falha por trás de "Produto Errado" e "Danificado"
- Recalcular o SLA de entrega com base na capacidade real observada (não na meta teórica de 90%)
- Monitorar OTD e taxa de devolução mensalmente após as primeiras ações corretivas
- Adicionar dados de custo operacional para calcular o ROI completo de melhorias no processo
