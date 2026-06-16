<p align="center">
  <img src="https://img.shields.io/badge/CEUB-Centro%20Universit%C3%A1rio%20de%20Bras%C3%ADlia-3D0E45?style=for-the-badge&labelColor=3D0E45&color=EC0C8D" />
</p>

<h1 align="center"> Readmissão Hospitalar e Risco Renal em Pacientes Diabéticos</h1>

<p align="center">
  <b>Projeto Final — Business Intelligence II</b><br>
  Centro Universitário de Brasília (CEUB) · 2026
</p>

---

## Autoras

| Nome | E-mail |
|---|---|
| Geovanna dos Santos Benedito | geovanna.sb@sempreceub.com |
| Sophia Silva Melo | sophia.silva@sempreceub.com |

---

## Sobre o projeto

Este projeto simula a atuação de uma equipe de Business Intelligence dentro do **Hospital Analytics Center (HAC)**, uma unidade fictícia de inteligência em dados clínicos. O objetivo é responder à seguinte pergunta de negócio:

> **Quais fatores clínicos e operacionais estão associados à readmissão hospitalar de pacientes diabéticos em até 30 dias, e como esses padrões evoluíram ao longo de 10 anos, com atenção especial ao subgrupo com indícios de doença renal crônica (DRC)?**

---

## Dataset utilizado

| Campo | Informação |
|---|---|
| Nome | Diabetes 130-US Hospitals for Years 1999–2008 |
| Fonte | [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008) |
| DOI | [10.24432/C5230J](https://doi.org/10.24432/C5230J) |
| Licença | Creative Commons Attribution 4.0 (CC BY 4.0) |
| Volume | 101.766 internações · 50+ variáveis |

Mais detalhes sobre o tratamento dos dados em [`data/README.md`](data/README.md).

---

## Principais KPIs

| KPI | Valor encontrado |
|---|---|
| Taxa de Readmissão em 30 dias (geral) | **11,16%** |
| Taxa de Readmissão em 30 dias (pacientes com Risco DRC) | **15,60%** |
| Proporção de internações com indício de Risco DRC | **9,23%** |
| Tempo médio de internação | **4,40 dias** |
| Variação anual de internações (YoY) | ~11% (exemplo) |

Detalhamento completo em [`docs/kpis_okrs.md`](docs/kpis_okrs.md).

---

## Modelo de Dados (Star Schema)

```
                    Dim_Calendario
                          │
Dim_Paciente ────── Fato_Internacao ────── Dim_Admissao
                          │
              ┌───────────┴───────────┐
         Dim_Clinico              Dim_Tratamento
```

- **1 Tabela Fato**: `Fato_Internacao` — granularidade de 1 linha por internação
- **5 Dimensões**: Paciente, Admissão, Clínico, Tratamento e Calendário (esta última construída em DAX)

Decisões de modelagem detalhadas em [`docs/decisoes_de_modelagem.md`](docs/decisoes_de_modelagem.md).
Diagrama visual em [`docs/modelo_dimensional.png`](docs/modelo_dimensional.png).

---

## Row-Level Security (RLS)

O modelo conta com 2 papéis de segurança:

| Papel | Visão |
|---|---|
| Diretor Clínico | Acesso total a todos os dados |
| Gerente de Qualidade | Visualiza apenas internações com readmissão em até 30 dias |

Detalhes e prints de teste em [`docs/rls.md`](docs/rls.md).

---

## Como abrir o projeto

1. Clone este repositório:
   ```
   git clone https://github.com/geovannadsb/ProjetoFinal_BI_II
   ```
2. Abra a pasta `pbip/` no **Power BI Desktop** (versão de junho/2026 ou mais recente)
3. Abra o arquivo `.pbip` — o Power BI vai carregar automaticamente o modelo, as medidas e os relatórios

---

## Estrutura do repositório

```
.
├── data/
│   └── README.md
├── docs/
│   ├── decisoes_de_modelagem.md
│   ├── kpis_okrs.md
│   ├── problema_de_negocio.md
│   ├── rls.md
│   └── modelo_dimensional.png
├── pbip/
│   └── diabetes_readmissao_bi.pbip + pastas associadas
├── apresentacao/
│   └── slides.pdf
└── README.md
```

---

<p align="center">
  <sub>Disciplina: Business Intelligence II · Professor: MsC Pedro H. Mello · CEUB · 2026</sub>
</p>
