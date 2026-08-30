# Curso GitHub Actions - Prof. Ieso Dias

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

Uma estratégia do GitHub Actions que permite executar o mesmo job várias vezes (paralelamente), utilizando diferentes combinações de configurações definidas em uma matriz (matrix).

Elas podem ser visualizadas na aba de Actions, no repositório.

> Útil para testar aplicações em diferentes versões, sistemas operacionais, ambientes ou configurações, sem precisar duplicar o job no workflow.

Por exemplo, uma matriz pode combinar:
```YAML
runs-on: ${{ matrix.os }}
strategy:
    fail-fast: false # Não para se um job falhar (o padrão é true)
    matrix:
    os: [ubuntu-latest, windows-latest]
    node: [20, 22]
```
Podemos ter flags para **incluir** uma configuração específica (`include`), ou **excluir** combinações problemáticas (`exclude`).

Isso gera 4 execuções automaticamente:
- Ubuntu + Node 20
- Ubuntu + Node 22
- Windows + Node 20
- Windows + Node 22

**Ideia principal**: definir as variações uma vez e deixar o GitHub Actions gerenciar as execuções.

> **Documentação**: [Matrix](https://docs.github.com/pt/actions/how-tos/write-workflows/choose-what-workflows-do/run-job-variations) e [Concorrência](https://docs.github.com/pt/actions/how-tos/write-workflows/choose-when-workflows-run/control-workflow-concurrency)

### Lab 07 - Workflow: Steps Condicionais

Controlar qual step roda, com base num comando condicional `if`.

Não só isso, como também condições no disparo daquele workflow, na propriedade `workflow_dispatch`. Utilizando uma entrada para valores que o usuário pode enviar (por meio dos `inputs`).
- Assim, é possível controlar algumas condições ou jobs a serem executados, ou não, além de escolher um ambiente, por exemplo.

> Útil quando manipulamos valores definidos como variáveis do ambiente, ou do próprio objeto `github`, como `github.event` ou `github.ref` por exemplo.

---

### Lab 08 - Workflow: Variáveis

---

### Lab 09 - Workflow: GitHub Secrets

---

### Lab 10 - Workflow: Variáveis multi-ambiente

---

### Lab 11 - Workflow: Contexto e Expressões