# Sobre o Dataset

## Identificação

| Campo | Informação |
|---|---|
| Nome | Diabetes 130-US Hospitals for Years 1999–2008 |
| Fonte original | Health Facts Database — Cerner Corporation (Kansas City, MO) |
| Repositório acadêmico | [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008) |
| DOI | [10.24432/C5230J](https://doi.org/10.24432/C5230J) |
| Licença | Creative Commons Attribution 4.0 International (CC BY 4.0) |
| Período coberto | 1999 a 2008 (10 anos de registros clínicos) |
| Volume | 101.766 registros de internações hospitalares · 50+ variáveis |

## Arquivos utilizados

- **`diabetic_data.csv`** — tabela principal, com uma linha por internação
- **`IDS_mapping.csv`** — dicionário de tradução para os códigos de `admission_type_id`, `discharge_disposition_id` e `admission_source_id`

## Tratamento aplicado (resumo)

Todo o tratamento foi feito em Power Query e está documentado em detalhe em [`../docs/decisoes_de_modelagem.md`](../docs/decisoes_de_modelagem.md). Resumo:

- Colunas `weight` e `payer_code` foram removidas (>95% de valores ausentes)
- Todas as colunas foram renomeadas para português (snake_case / Title Case)
- Valores de colunas categóricas (gênero, raça, medicamentos, resultados de exames, readmissão) foram traduzidos para português
- Foram criadas duas colunas derivadas centrais para a análise:
  - `Readmissao_30d` (flag 1/0)
  - `Risco_DRC` (flag 1/0, baseado em códigos CID renais presentes nos diagnósticos)
- Os códigos de admissão/alta com 26–30 categorias foram agrupados em categorias menores com significado de negócio

## Sobre a dimensão temporal (importante)

O dataset **não contém nenhuma coluna de data**. A documentação oficial do UCI informa apenas que os dados cobrem o período de **1999 a 2008**, sem indicar a data exata de cada internação, provavelmente por motivos de anonimização clínica.

Para viabilizar a `Dim_Calendario` (exigência do projeto) e o uso de funções de Time Intelligence em DAX, foi criada uma coluna `ano` no Power Query, **distribuindo os 101.766 registros de forma proporcional e sequencial entre 1999 e 2008** (~10.176 registros por ano).

Essa coluna é **sintética**, não reflete a data real de cada internação, apenas viabiliza a análise de tendência ano a ano de forma didaticamente coerente com a estrutura exigida pelo projeto. Essa limitação é citada de forma transparente na documentação.

---

*Disciplina: Business Intelligence II · CEUB · Professor: MsC Pedro H. Mello*
