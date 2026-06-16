# KPIs e OKRs

Os valores de referência abaixo foram extraídos diretamente das medidas DAX do modelo, construído sobre a base **Diabetes 130-US Hospitals (1999–2008)**, com 101.766 internações.

---

## KPIs (Key Performance Indicators)

### KPI 1 — Taxa de Readmissão em 30 Dias

| Campo | Valor |
|---|---|
| **Fórmula** | `DIVIDE( CALCULATE(COUNTROWS(Fato_Internacao), Readmissao_30d = 1), Total de Internações )` |
| **Unidade** | Percentual (%) |
| **Valor atual** | **11,16%** |
| **Meta** | Reduzir para abaixo de 10% — referência usada por hospitais americanos no Hospital Readmissions Reduction Program (HRRP) |
| **Justificativa** | É o indicador central de qualidade assistencial do projeto. Readmissões em curto prazo geralmente indicam alta precoce, tratamento incompleto ou falha no acompanhamento pós-alta, gerando custo extra e risco ao paciente. |

### KPI 2 — Proporção de Internações com Risco Renal (DRC)

| Campo | Valor |
|---|---|
| **Fórmula** | `DIVIDE( CALCULATE(COUNTROWS(Fato_Internacao), Risco_DRC = 1), Total de Internações )` |
| **Unidade** | Percentual (%) |
| **Valor atual** | **9,23%** |
| **Meta** | Monitorar a tendência ao longo dos anos, qualquer aumento sustentado indica necessidade de ação preventiva na rede |
| **Justificativa** | Identifica o tamanho do subgrupo de pacientes diabéticos com diagnóstico associado a doença renal (CID 585.x, 403.x ou 250.4x). Esse grupo é o foco da análise de risco do projeto. |

### KPI 3 — Taxa de Readmissão entre Pacientes com Risco DRC

| Campo | Valor |
|---|---|
| **Fórmula** | `CALCULATE( % Readmissão em 30 dias, Risco_DRC = 1 )` |
| **Unidade** | Percentual (%) |
| **Valor atual** | **15,60%** |
| **Meta** | Reduzir a diferença em relação à taxa geral (11,16%), meta de aproximação para 12% em 18 meses |
| **Justificativa** | Este é o KPI que valida a hipótese central do projeto: pacientes com risco renal são readmitidos com frequência ~4,4 pontos percentuais maior que a média geral — quase 40% mais. É o principal indicador para justificar intervenções direcionadas. |

### KPI 4 — Tempo Médio de Internação

| Campo | Valor |
|---|---|
| **Fórmula** | `AVERAGE( Dias_de_Internação )` |
| **Unidade** | Dias |
| **Valor atual** | **4,40 dias** |
| **Meta** | Manter abaixo de 4,5 dias — qualquer aumento sustentado pode indicar agravamento do perfil clínico dos pacientes internados |
| **Justificativa** | Tempo de internação está diretamente ligado a custo hospitalar e a risco de complicações adicionais. É um KPI operacional que complementa a visão clínica dos outros indicadores. |

> **Nota de leitura**: 4,40 dias é uma **média** entre todas as 101.766 internações — não o tempo de uma internação específica (que é sempre um número inteiro de dias). É o mesmo princípio de indicadores como "número médio de filhos por família": o valor agregado pode ter casas decimais mesmo que cada caso individual seja um número inteiro. Esse é o formato padrão usado por hospitais reais para reportar este indicador (ex.: *Average Length of Stay*).

### KPI 5 — Variação Anual de Internações (YoY)

| Campo | Valor |
|---|---|
| **Fórmula** | `DIVIDE( Total de Internações − Internações Ano Anterior, Internações Ano Anterior )` |
| **Unidade** | Percentual (%) — variação ano a ano |
| **Valor de referência** | Exemplo observado: +11,12% em um dos anos do período 2000–2008 |
| **Meta** | Variação estável (entre -5% e +10%), picos acima disso acionam revisão de capacidade hospitalar |
| **Justificativa** | Permite à diretoria identificar tendências de crescimento ou queda na demanda por internações relacionadas a diabetes ao longo dos 10 anos do dataset, apoiando planejamento de capacidade. |

---

## OKRs (Objectives and Key Results)

Cada Key Result está vinculado a pelo menos um dos KPIs acima — indicado entre parênteses.

### OKR 1 — Reduzir o impacto da readmissão hospitalar em pacientes diabéticos com risco renal

- **KR1**: Reduzir a taxa de readmissão em 30 dias entre pacientes com Risco DRC de 15,60% para 12% até dezembro de 2027 *(KPI 3)*
- **KR2**: Aumentar para 90% a proporção de internações de pacientes com Risco DRC que realizaram o exame de HbA1c durante a internação, até dezembro de 2027 *(KPI 2)*
- **KR3**: Reduzir o tempo médio de internação geral de 4,40 para 4,0 dias até dezembro de 2027, sem aumento da taxa de readmissão *(KPI 4 e KPI 1)*
- **KR4**: Reduzir em 2 pontos percentuais a diferença entre a taxa de readmissão geral e a taxa entre pacientes com Risco DRC até dezembro de 2027 *(KPI 1 e KPI 3)*

### OKR 2 — Aprimorar o monitoramento de fatores de risco renal na população diabética internada

- **KR1**: Manter 100% das internações classificadas quanto à presença do indicador `Risco_DRC`, garantindo rastreabilidade contínua via diagnósticos CID *(KPI 2)*
- **KR2**: Reduzir a proporção geral de internações com Risco DRC de 9,23% para abaixo de 8% até dezembro de 2028, por meio de ações preventivas *(KPI 2)*
- **KR3**: Realizar revisão trimestral da variação YoY de internações para apoiar o planejamento de capacidade hospitalar *(KPI 5)*
- **KR4**: Apresentar à diretoria clínica, semestralmente, o cruzamento entre destino da alta e taxa de readmissão para identificar pontos de falha no cuidado pós-alta *(KPI 1)*

---

*Disciplina: Business Intelligence II · CEUB · Professor: MsC Pedro H. Mello*
