# Curso GitHub Actions - Prof. Ieso Dias

> **Curso na Udemy** - [Link](https://www.udemy.com/course/github-actions-guia-completo-do-zero-ao-deploy/)

> Material no **Notion**!

## Sobre o Curso

O curso **GitHub Actions: Guia Completo - Do Zero ao Deploy** oferece uma formação prática e abrangente sobre automação de CI/CD usando GitHub Actions. Você aprenderá desde os conceitos fundamentais ate a implementação de pipelines completas de deploy em produção.

> **Repositório** com os laboratórios respondidos - [Link](https://github.com/iesodias/ghc-repo)

## Labs. - Colocando em Prática

### Lab 01 - Estrutura dos Workflows

Workflow básico sobre a sintaxe e estruturação de uma pipeline simples.

---

### Lab 02 - Workflow Trigger: Cron

Utilizamos a chave `cron`, aninhado ao tipo `schedule`, para determinar um tempo para aquele workflow executar.

> Horário **UTC**

Site para transformar um tempo estipulado em uma expressão cron: [Crontab Guru](https://crontab.guru/)

**Expressão**: `minute hour day month weekday`

Possíveis causas para o schedule não ter sido executado:
- **Repositório privado** => Schedule não funciona
- **Branch diferente de main** => Schedule não funciona  
- **Sintaxe cron errada** => Verifique em [crontab.guru](https://crontab.guru)
- **Horário UTC/Brasília** => GitHub usa UTC, ajustar +3 horas
- **Delay do GitHub** => Pode atrasar até 10-15 minutos

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

Nesse lab. vamos utilizar das **variáveis de ambiente**, um mecanismo para evitar a repetição de valores no workflow, e ter o maior controle deles.

Uso da propriedade `env`, e os nomes das variáveis em MAIÚSCULO e `snake_case` (no formato `chave: valor`).

**Níveis**:
1. <u>Workflow</u> - nível global;
2. <u>Jobs</u> - válidas apenas naquele bloco de execução;
3. <u>Steps</u> - válidas apenas naquele comando específico.

**Precedência de variáveis**: Se você definir uma variável com o mesmo nome em diferentes níveis, a ordem de precedência é:
1. **Step** (maior prioridade)
2. **Job**
3. **Workflow** (menor prioridade)

---

### Lab 09 - Workflow: GitHub Secrets

Um recurso especial para **proteger informações sensíveis** dentro do workflow.

Definimos as informações no próprio repositório (na aba de "Settings" e na seção "Security and quality" -> "Actions" -> "Repository secrets").
- Podemos incluir dados como segredos (_secrets_) ou propriamente variáveis (_variables_).
    - <u>Secrets</u>: Para dados sensíveis, são mascarados nos logs, não podem ser lidos após criação. Referenciamos por meio do objeto `secrets.<name>`.
    - <u>Variables</u>: Para configurações não sensíveis, são visíveis nos logs, podem ser lidas

> Mesmo que você tente imprimir o valor segredo, o GitHub mascara automaticamente.

Você não consegue visualizar um valor definido como segredo, e sim, somente **atualizá-lo** (_update_).

---

### Lab 10 - Workflow: Variáveis multi-ambiente

Entender como funciona as **variáveis de repositório** e **variáveis de ambiente específicas** (configuradas por environment como `dev`, `staging` e `production`).

> Clicaremos na aba de "Variables", e não "Secrets".

**Importante**: variáveis de repositório são acessíveis em todos os workflows e jobs, mas podem ser sobrescritas por variáveis de ambiente específicas.
- Deixamos explícito qual ambiente queremos executar determinado job, por meio da propriedade `environment`. 

Lembre-se de definir as variáveis no repositório, por **ambiente**.
- Por ambiente, clicamos na seção de "Manage environment variables" -> "New environment". 

Quando há variáveis com o mesmo nome, a precedência é:
1. **Variáveis de Environment** (mais alta prioridade)
2. **Variáveis de Repositório** (prioridade média)
3. **Variáveis de Organização** (prioridade baixa)

**Exemplo prático:**
- Se você criar `DEFAULT_REGION` como variável de repositório com valor `us-east-1`
- E criar `DEFAULT_REGION` no environment `production` com valor `eu-west-1`
- O job usando `environment: production` vai usar `eu-west-1` (sobrescreve)

#### Proteções de Environment

Os environments permitem configurar:
- **Required reviewers:** Aprovação manual antes do deploy
- **Wait timer:** Atraso antes de iniciar o deploy
- **Deployment branches:** Restringir quais branches podem fazer deploy
- **Environment secrets:** Secrets específicos por ambiente (além de variáveis)

Essas proteções são essenciais em ambientes de produção!

---

### Lab 11 - Workflow: Contexto e Expressões
