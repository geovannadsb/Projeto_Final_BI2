# Decisões de Modelagem

Este documento explica o porquê de cada decisão tomada na construção do modelo dimensional, para que qualquer pessoa entenda a lógica por trás do star schema.

---

## 1. Por que Star Schema (e não Snowflake ou tabela única)

Optamos por um star schema clássico: 1 tabela fato cercada por dimensões diretamente relacionadas a ela, sem níveis intermediários (snowflake).

Motivos:
- Simplifica os relacionamentos (todos 1:N, fluindo da dimensão para a fato)
- Melhora a performance do DAX, menos saltos entre tabelas
- É o padrão recomendado pelo próprio Power BI e exigido pelo enunciado do projeto

Uma tabela única ("flat") foi descartada porque misturaria granularidades diferentes (dados do paciente, da admissão, do diagnóstico e da internação na mesma linha), dificultando filtros e Time Intelligence.

---

## 2. Granularidade da tabela fato

`Fato_Internacao` tem granularidade de **1 linha = 1 internação** (`encounter_id`).

Essa escolha foi feita porque é o nível mais detalhado disponível no dataset e porque o problema de negócio (readmissão em 30 dias) é definido por internação, não por paciente, um mesmo paciente pode ter internações com desfechos diferentes.

---

## 3. As dimensões — o que cada uma representa e por quê

| Tabela | O que descreve | Por que é uma dimensão |
|---|---|---|
| Dim_Paciente | Quem foi internado (gênero, raça, faixa etária) | Atributos que descrevem a pessoa, não o evento |
| Dim_Admissao | Como o paciente entrou e saiu (tipo, origem, destino da alta) | Atributos categóricos sobre o fluxo de entrada/saída |
| Dim_Clinico | Diagnósticos (CID), exames de glicose/HbA1c, especialidade médica | Contexto clínico da internação |
| Dim_Tratamento | Medicamentos prescritos e se houve mudança de medicação | Perfil farmacológico — dimensão conformada, reutilizável em outros Data Marts do hospital |
| Dim_Calendario | Ano, trimestre, mês, dia da semana | Dimensão de tempo - obrigatória para Time Intelligence |

---

## 4. Surrogate Keys (SK)

Todas as dimensões receberam uma surrogate key (`sk_paciente`, `sk_admissao`, `sk_clinico`, `sk_tratamento`) gerada via Coluna de Índice no Power Query.

Por quê:
- O dataset original não tem uma chave própria para "linha de admissão" ou "linha de diagnóstico", usamos `ID_do_encontro` como base, mas a SK garante uma chave numérica simples e sequencial, independente da fonte
- É uma boa prática de Data Warehousing: a estrutura do DW não deve depender de identificadores do sistema de origem

---

## 5. `ID_do_encontro` como dimensão degenerada

`encounter_id` (renomeado para `ID_do_encontro`) não tem tabela própria, ele fica diretamente na `Fato_Internacao`.

Por quê: ele identifica o evento (a internação) de forma única, mas não tem nenhum atributo descritivo próprio, é só um número de protocolo. Criar uma tabela só para ele seria redundante. Esse é o conceito clássico de dimensão degenerada.

---

## 6. Colunas derivadas criadas no Power Query

### Readmissao_30d
Transforma o texto da coluna `Readmissao` (`<30`, `>30`, `NO`) em uma flag numérica (1 ou 0), onde 1 = readmitido em menos de 30 dias.

Motivo: números permitem cálculos de proporção (%) diretamente nas medidas DAX; texto não.

### Risco_DRC
Coluna calculada que verifica se algum dos três diagnósticos (`Diagnostico_Primario`, `Diagnostico_Secundario`, `Diagnostico_Terciario`) começa com um código CID associado a doença renal:

- 585.x → Doença Renal Crônica
- 403.x → Hipertensão com doença renal
- 250.4x → Diabetes com complicação renal

Motivo: o dataset não possui uma coluna direta de "diagnóstico de DRC". Essa coluna derivada é o que torna possível a análise central do projeto — relacionar diabetes, readmissão e risco renal.

> Limitação documentada: esse indicador é um proxy de risco baseado em código de diagnóstico, não um diagnóstico clínico definitivo. Isso é citado no problema de negócio para transparência acadêmica.

---

## 7. Agrupamento de categorias (destino_alta e origem_admissao)

As colunas originais `ID_disposição_de_alta` (30 categorias) e `ID_fonte_de_admissao` (26 categorias) foram agrupadas em categorias menores com significado de negócio, em vez de traduzidas uma a uma.

Exemplos de agrupamento:
- Códigos de óbito (11, 19, 20, 21) → categoria única "Óbito"
- Códigos "Not Available", "NULL", "Not Mapped", "Unknown" → categoria única "Não informado"

Motivo: 30 categorias diferentes tornariam qualquer gráfico ilegível. Agrupar por significado clínico/operacional é uma prática real de modelagem de BI e torna o dashboard mais útil para a tomada de decisão.

---

## 8. A Dim_Calendario e a coluna ano sintética

O dataset original não contém nenhuma coluna de data, apenas o intervalo geral de 1999 a 2008 é informado na documentação do UCI.

Para viabilizar a dimensão Calendário (exigência obrigatória do projeto) e o uso de funções de Time Intelligence, foi criada no Power Query uma coluna `ano`, distribuindo os 101.766 registros proporcionalmente entre 1999 e 2008 (~10.176 registros por ano).

A partir dessa coluna, foi gerada uma coluna `data_completa` (01/01/AAAA), relacionada à Dim_Calendario, uma tabela completa de dias gerada em DAX via `CALENDAR()`, marcada como tabela de datas oficial do modelo.

> Limitação documentada: a granularidade temporal real do dataset é anual, não diária. A distribuição dos registros entre os anos é sintética e proporcional, criada exclusivamente para fins didáticos de Time Intelligence, não representa a data real de cada internação. Essa limitação é registrada também em `data/README.md`.

---

## 9. Colunas removidas

| Coluna | Motivo da remoção |
|---|---|
| `weight` (peso) | Mais de 95% dos valores eram `?` (ausentes) sem valor analítico |
| `payer_code` (código de pagador) | Mesma situação, predominantemente `?` |

---

## 10. Relacionamentos

Todos os relacionamentos seguem o padrão **1 (dimensão) : N (fato)**, com direção de filtro única (da dimensão para a fato). Nenhum relacionamento muitos-para-muitos foi utilizado.

---

*Disciplina: Business Intelligence II · CEUB · Professor: MsC Pedro H. Mello*
