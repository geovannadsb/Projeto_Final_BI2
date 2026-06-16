# Problema de Negócio

## Contexto

A organização fictícia adotada para este projeto é o **Hospital Analytics Center (HAC)**, uma unidade de inteligência em dados clínicos vinculada a uma rede hospitalar. O HAC é responsável por transformar dados de prontuários eletrônicos em indicadores que apoiem decisões médicas, operacionais e estratégicas.

O contexto desta análise é o gerenciamento de pacientes diabéticos internados: um problema crítico de saúde pública e de custo hospitalar, dado que pacientes diabéticos têm maiores taxas de complicações, readmissões e permanências prolongadas.

## Problema de Negócio

A diretoria clínica do HAC identificou que pacientes diabéticos readmitidos em menos de 30 dias após a alta representam um custo elevado e um indicador de falha no cuidado. A equipe de BI foi acionada para responder à seguinte pergunta central:

> **Quais fatores clínicos e operacionais estão associados à readmissão hospitalar de pacientes diabéticos em até 30 dias, e como esses padrões evoluíram ao longo dos 10 anos de dados?**

## Stakeholders

| Perfil | Interesse principal no dashboard |
|---|---|
| Diretor Clínico | Visão executiva: taxa de readmissão, tendência anual e custo associado |
| Gerente de Qualidade Assistencial | Quais especialidades e tipos de admissão geram mais readmissões |
| Equipe Médica | Perfil clínico dos pacientes de alto risco (medicamentos, diagnósticos) |
| Financeiro | Tempo médio de internação e número de procedimentos por readmissão |

## Hipóteses Iniciais

- Pacientes com mais diagnósticos secundários têm maior probabilidade de readmissão em 30 dias.
- Pacientes mais idosos e com hipertensão associada apresentam internações mais longas.
- A não realização do exame de HbA1c durante a internação está correlacionada a maiores taxas de readmissão.
- O número de medicamentos prescritos varia ao longo dos 10 anos do dataset, refletindo evolução nos protocolos clínicos.
- Pacientes admitidos por emergência têm perfil clínico mais grave que os admitidos eletivamente.
- **Pacientes com indícios de doença renal crônica (DRC) têm taxa de readmissão em 30 dias maior que a média geral.**

> A última hipótese foi **confirmada** durante a construção do modelo: a taxa de readmissão entre pacientes com indício de risco renal é de **15,60%**, contra **11,16%** na população geral, uma diferença de quase 40%.

## Critério de Sucesso

O projeto será considerado bem-sucedido quando o dashboard permitir que a diretoria clínica identifique, em menos de 5 minutos de navegação, os principais perfis de pacientes com alto risco de readmissão — com drill-down por ano, tipo de admissão, especialidade e perfil medicamentoso — e possa fundamentar uma decisão de intervenção baseada nos KPIs apresentados.

---

*Disciplina: Business Intelligence II · CEUB · Professor: MsC Pedro H. Mello*
