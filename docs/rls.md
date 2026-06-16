# Row-Level Security (RLS)

## Papéis criados

### 1. Diretor Clínico

- **Acesso**: total; vê todos os dados do modelo, sem restrições
- **Filtro DAX**: nenhum filtro aplicado (papel sem restrição)
- **Justificativa**: a diretoria precisa da visão completa para decisões estratégicas e para auditar os outros papéis

### 2. Gerente de Qualidade Assistencial

- **Acesso**: apenas internações com readmissão em até 30 dias
- **Filtro DAX aplicado na tabela `Fato_Internacao`**:
  ```dax
  [Readmissao_30d] = 1
  ```
- **Justificativa**: o time de qualidade assistencial atua especificamente sobre os casos de readmissão precoce, são esses os casos que precisam de investigação e plano de ação. Restringir a visão a esse subconjunto mantém o foco do papel e evita exposição de dados de pacientes fora do escopo de atuação desse time.

---

## Como o RLS foi testado

Para cada papel, foi utilizada a opção **"View as roles"** (Exibir como funções) no Power BI Desktop, na aba **Modelagem**.

### Teste — Diretor Clínico

*(print mostrando o dashboard com todos os dados visíveis)*

### Teste — Gerente de Qualidade Assistencial

*(print mostrando o dashboard filtrado, exibindo apenas internações com `Readmissao_30d = 1`)*

---

## Observação

Os papéis foram aplicados sobre a tabela `Fato_Internacao`, que é o ponto central do modelo, como todas as dimensões se relacionam a ela com filtro de dimensão → fato, qualquer restrição na fato automaticamente limita o que aparece em todos os visuais do relatório.

---

*Disciplina: Business Intelligence II · CEUB · Professor: MsC Pedro H. Mello*
