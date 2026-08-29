u# Curso GitHub Actions - Prof. Ieso Dias

> **Curso na Udemy** - [Link](https://www.udemy.com/course/github-actions-guia-completo-do-zero-ao-deploy/)

> Material no **Notion**!

## Labs. - Colocando em Prática

### Lab 01 - Estrutura dos Workflows

Workflow básico sobre a sintaxe e estruturação de uma pipeline simples.

---

### Lab 02 - Workflow Trigger: Cron

Utilizamos a chave `cron`, aninhado ao tipo `schedule`, para determinar um tempo para aquele workflow executar.

> Horário **UTC**

Site para transformar um tempo estipulado em uma expressão cron: [Crontab Guru](https://crontab.guru/)

**Expressão**: `minute hour day month weekday`

---

### Lab 03 - Workflow Trigger: Branch

A ideia desse lab. é fazer com que o pipeline só execute a partir de mudanças e em branches **específicas**.

Utilizamos a chave `branches` e `paths`

---

### Lab 04 - Workflow Trigger: Issue

Automatizar ações, quando alguém interage com alguma **Issue**, de acordo com alguns tipos especificados.

Útil para automatizar respostas e tratamentos em issues criadas por outros usuários, até externos do projeto (comunidade).

Utilizamos a chave `issues` e `types` (`opened, edited, closed, labeled`).

> Lembre-se de dar permissão de escrita (`permissions: issues: write`) ao `GITHUB_TOKEN`.

---

### Lab 05 - Workflow: Jobs Paralelos

Uma técnica muito utilizada para etapas que não são dependentes entre si, consigam ser executadas em **paralelo**, tornando o workflow mais eficiente.

> Por padrão, os jobs são executados **paralelamente**, ou sequencial mediante dependências (`needs`).

- <u>Considerações de performance</u>:
    - Paralelizar reduz tempo crítico de **feedback** (CI), mas aumenta consumo de minutos (caso de runners hospedados).
        - Equilibrar **tempo e custo**.
    - Jobs muito curtos (segundos) podem sofrer mais overhead de **provisionamento** que ganho real — avaliar junção.

---

### Lab 06 - Workflow: Matrix Strategy

---

### Lab 07 - Workflow: Steps condicionais

---

### Lab 08 - Workflow: Variáveis

---

### Lab 09 - Workflow: GitHub Secrets

---

### Lab 10 - Workflow: Variáveis multi-ambiente

---

### Lab 11 - Workflow: Contexto e Expressões