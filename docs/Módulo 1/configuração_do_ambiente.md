# Setup do Ambiente — QA com Claude Code

## Todos os arquivos que você precisa criar

**Curso:** IA para Analistas de QA · Centro de Inovações Edge / UFAL

---

## Como usar este arquivo

Crie cada arquivo listado abaixo copiando exatamente o conteúdo indicado.
A estrutura completa ao final do setup:

```
~/.claude/                             ← global, vale para todos os projetos
├── CLAUDE.md
├── settings.json
├── hooks/
│   ├── user-prompt-submit.sh
│   ├── pre-tool-call.sh
│   ├── post-tool-call.sh
│   └── stop.sh
├── skills/
│   ├── gherkin-standards/SKILL.md
│   ├── qase-structure/SKILL.md
│   ├── bug-report/SKILL.md
│   ├── test-case-quality/SKILL.md
│   ├── acceptance-criteria/SKILL.md
│   └── edge-qa-workflow/SKILL.md
└── commands/
    └── qa/
        ├── test-from-us.md
        ├── bug-analysis.md
        ├── regression-check.md
        ├── sprint-test-plan.md
        ├── doc-test-update.md
        ├── review-test-suite.md
        └── close-us.md

[raiz do projeto]/
└── CLAUDE.md                          ← seção QA adicionada aqui
```

> ⚠️ O `CLAUDE.md` do projeto **não deve ser commitado** com as alterações
> do QA. Adicione-o ao `.gitignore` pessoal (`~/.config/git/ignore`) ou
> desfaça as alterações antes de commitar.

---

## 1. CLAUDE.md do Projeto

O `CLAUDE.md` na raiz do projeto fornece ao Claude contexto específico
do sistema que está sendo testado: chave do Jira, projeto do Qase, épicos
ativos e URLs de ambiente. Carrega automaticamente toda vez que você
inicia o Claude Code nessa pasta.

---

### Caso A — Projeto que já tem CLAUDE.md (criado pelos devs)

Abra o arquivo existente e **acrescente a seção QA ao final**:

```bash
nano CLAUDE.md
# Vá até o final do arquivo e cole o bloco abaixo
```

```markdown
## QA — Contexto do Projeto

### Identificação

- Projeto Jira: AIQA
- URL Jira: https://kodercat.atlassian.net
- Projeto Qase: AIQA
- Espaço Confluence: GD (Gestor de Documentos)

### Épicos Ativos

- [EP1] Gestão de Documentos
- [EP2] Controle de Acesso
- [EP3] Fluxo de Aprovação
  (atualize conforme os épicos da sprint atual)

### Ambientes

- Homologação: https://htmdoc-hom.edge.ufal.br
- UAT: https://htmdoc-uat.edge.ufal.br

### Comportamento esperado para este projeto

- Sempre buscar a issue no Jira antes de gerar casos de teste
- Sempre verificar a suíte no Qase antes de criar uma nova
- Ao criar casos de teste, usar o projeto Qase: AIQA
- Ao criar bugs, linkar à US correspondente no Jira
```

---

### Caso B — Projeto que ainda não tem CLAUDE.md

Crie o arquivo do zero na raiz do projeto:

```bash
nano CLAUDE.md
```

```markdown
# CLAUDE.md — [Nome do Projeto]

## Visão Geral

[Descrição breve do sistema — preenchida pelos devs]

## Stack

[Tecnologias utilizadas — preenchida pelos devs]

## Comandos úteis

[Scripts de build, test, lint — preenchidos pelos devs]

---

## QA — Contexto do Projeto

### Identificação

- Projeto Jira: AIQA
- URL Jira: https://kodercat.atlassian.net
- Projeto Qase: AIQA
- Espaço Confluence: GD (Gestor de Documentos)

### Épicos Ativos

- [EP1] Gestão de Documentos
- [EP2] Controle de Acesso
- [EP3] Fluxo de Aprovação
  (atualize conforme os épicos da sprint atual)

### Ambientes

- Homologação: https://htmdoc-hom.edge.ufal.br
- UAT: https://htmdoc-uat.edge.ufal.br

### Comportamento esperado para este projeto

- Sempre buscar a issue no Jira antes de gerar casos de teste
- Sempre verificar a suíte no Qase antes de criar uma nova
- Ao criar casos de teste, usar o projeto Qase: AIQA
- Ao criar bugs, linkar à US correspondente no Jira
```

---

## 2. CLAUDE.md Global

```bash
nano ~/.claude/CLAUDE.md
```

```markdown
# Padrões de QA — Centro de Inovações Edge / UFAL

## Gherkin

- Keywords SEMPRE em inglês: Given, When, Then, And, But
- Restante da frase SEMPRE em português
- Correto: "Given que estou logado como gerente de projetos"
- Errado: "Dado que estou logado..." ou "Given I am logged in..."
- Um único comportamento por cenário
- Steps atômicos e verificáveis

## Nomenclatura de Casos de Teste

Formato: [US-XXX] – Caso XX: [descrição curta]
Exemplos:
[US-089] – Caso 01: Listar documentos com filtro ativo
[US-089] – Caso 02: Listar documentos sem permissão de visualização

## Severidade de Bugs

- Bloqueante: fluxo principal impossível de executar
- Alta: funcionalidade crítica com comportamento errado
- Média: bug com contorno disponível
- Baixa: cosmético / UX não crítico

## Protocolo de Reporte de Bug

Todo bug deve conter obrigatoriamente:

1. Passos para reproduzir (numerados)
2. Resultado atual
3. Resultado esperado
4. Evidências: print, vídeo ou log
5. Severidade

## Campos Obrigatórios em Casos de Teste no Qase

Ao criar casos de teste no Qase, sempre preencher:
- **description**: objetivo do teste e critério de aceitação de referência
- **preconditions**: estado necessário do sistema antes da execução
- **postconditions**: estado esperado do sistema após a execução

## Comportamento Padrão

- Sempre confirmar antes de criar, modificar ou excluir itens no Qase ou Jira
- Nunca assumir critérios de aceite que não estejam descritos na US
- Sempre perguntar sobre a US de referência antes de gerar casos de teste
```

---

## 3. Skills

```bash
mkdir -p ~/.claude/skills/gherkin-standards
mkdir -p ~/.claude/skills/qase-structure
mkdir -p ~/.claude/skills/bug-report
mkdir -p ~/.claude/skills/test-case-quality
mkdir -p ~/.claude/skills/acceptance-criteria
mkdir -p ~/.claude/skills/edge-qa-workflow
```

---

### 3.1 gherkin-standards

```bash
nano ~/.claude/skills/gherkin-standards/SKILL.md
```

````markdown
---
name: gherkin-standards
description: >
  Mandatory Gherkin standards for Edge QA team. Load this skill whenever the
  user is writing, generating, reviewing, or fixing any Gherkin syntax — including
  test cases, acceptance criteria, or BDD scenarios. Trigger phrases: "gerar casos
  de teste", "escrever cenário", "formato Gherkin", "Given When Then", "Dado Quando
  Então", "criar caso de teste", "escrever critério de aceitação em Gherkin",
  "revisar cenário", "corrigir Gherkin". Also load when user pastes Gherkin content
  for review or asks about BDD syntax.
---

# Padrões Gherkin — Edge / UFAL

## Regra fundamental

Keywords SEMPRE em inglês. Restante da frase SEMPRE em português.

## Keywords válidas

| Keyword | Uso                               |
| ------- | --------------------------------- |
| Given   | Estabelece o contexto inicial     |
| When    | Descreve a ação do usuário        |
| Then    | Descreve o resultado esperado     |
| And     | Continua um Given, When ou Then   |
| But     | Adiciona uma exceção ou restrição |

## Exemplo correto

```gherkin
Given que estou logado como analista de qualidade
And estou na tela de listagem de documentos
When eu aplico o filtro por tipo "Contrato"
Then apenas documentos do tipo "Contrato" devem ser exibidos
And o contador de resultados deve refletir a quantidade filtrada
```
````

## Erros comuns

| Errado                                           | Correto                    |
| ------------------------------------------------ | -------------------------- |
| `Dado que estou logado`                          | `Given que estou logado`   |
| `When I click the button`                        | `When eu clico no botão`   |
| `Given que estou logado e a tela está carregada` | Separar em `Given` e `And` |

## Regras de qualidade

- Um único comportamento por cenário
- Cada step deve ser atômico — faz uma única coisa
- `Then` sempre verificável de forma objetiva
- Evitar detalhes de implementação (UI pode mudar)
- Máximo recomendado: 3 Given, 2 When, 3 Then por cenário

````

---

### 3.2 qase-structure

```bash
nano ~/.claude/skills/qase-structure/SKILL.md
````

````markdown
---
name: qase-structure
description: >
  Mandatory Qase hierarchy and naming conventions for Edge QA team. Load this skill
  whenever the user is creating, naming, or organizing anything in Qase — suites,
  test cases, acceptance criteria sections, or test runs. Trigger phrases: "criar
  suíte", "criar caso no Qase", "organizar Qase", "estrutura do Qase", "suíte do
  épico", "[EP", "[US-", "subseção AC", "subseção TC", "hierarquia do Qase",
  "nomenclatura do caso", "nomear caso de teste". Also load when user asks how
  to name or structure anything inside Qase.
---

# Estrutura do Qase — Edge / UFAL

## Hierarquia obrigatória

```
Qase
└── Repositório de Testes
    └── Suíte: [EP1] Nome do Épico
        └── [US-XXX] – Nome curto da US
            ├── AC – Critérios de Aceitação
            │   ├── [US-XXX] – Critério 1: [descrição]
            │   └── [US-XXX] – Critério 2: [descrição]
            └── TC – Casos de Teste
                ├── [US-XXX] – Caso 01: [ação ou fluxo testado]
                └── [US-XXX] – Caso 02: [ação ou fluxo testado]
```

## Nomenclatura

**Suíte do Épico:** `[EP + número] Nome do Épico`
Exemplo: `[EP3] Gestão de Documentos`

**Subseção da US:** `[US-XXX] – Nome curto`
Exemplo: `[US-089] – Listagem de Documentos`

**Critério de Aceitação:** `[US-XXX] – Critério N: [descrição curta]`
Exemplo: `[US-089] – Critério 1: Filtro por tipo de documento`

**Caso de Teste:** `[US-XXX] – Caso XX: [ação ou fluxo testado]`
Exemplo: `[US-089] – Caso 01: Listar documentos com filtro ativo`

## Regras

- Criar a subseção `AC` antes da `TC`
- Registrar critérios de aceitação em `AC` antes de escrever os TCs
- Cada CA deve ter ao menos um TC correspondente em `TC`
- TCs cobrem: cenário feliz, cenário negativo e edge cases do critério
````

---

### 3.3 bug-report

```bash
nano ~/.claude/skills/bug-report/SKILL.md
```

```markdown
---
name: bug-report
description: >
  Mandatory bug report template and standards for Edge QA team. Load this skill
  whenever the user is creating, writing, reviewing, or improving a bug report.
  Trigger phrases: "reportar bug", "registrar bug", "abrir bug", "criar defeito",
  "reportar problema", "preencher bug", "severidade do bug", "passos para
  reproduzir", "resultado esperado", "resultado atual", "evidência do bug",
  "bug no Qase", "relatar falha". Also load when user describes a software defect
  and needs to document it.
---

# Template de Bug Report — Edge / UFAL

## Campos obrigatórios no Qase

**Título:** [Módulo] Descrição objetiva do problema
Exemplo: `[Documentos] Filtro por tipo não retorna resultados após segunda aplicação`

**Passos para reproduzir:**

1. [Passo numerado e específico]
2. [Passo numerado e específico]
3. [...]

**Resultado atual:** o que acontece de fato

**Resultado esperado:** o que deveria acontecer

**Evidências:** print, vídeo ou log anexado (obrigatório)

**Severidade:**

- Bloqueante — fluxo principal impossível de executar
- Alta — funcionalidade crítica com comportamento errado
- Média — bug com contorno disponível
- Baixa — cosmético / UX não crítico

## Regras

- Título objetivo e técnico — nunca "botão não funciona"
- Passos reproduzíveis por qualquer pessoa sem contexto adicional
- Evidência é obrigatória — sem print/vídeo/log o bug não está completo
- Bugs sincronizam automaticamente com o Jira — não criar duplicatas
- Bug bloqueante ou alto → US retorna para "For Planning" no Jira
```

---

### 3.4 test-case-quality

```bash
nano ~/.claude/skills/test-case-quality/SKILL.md
```

```markdown
---
name: test-case-quality
description: >
  Quality checklist for test cases at Edge QA team. Load this skill whenever the
  user is reviewing, auditing, or assessing the completeness of test cases.
  Trigger phrases: "revisar casos de teste", "avaliar cobertura", "checar qualidade",
  "verificar casos", "melhorar caso de teste", "cobertura está boa?", "faltou
  algum cenário?", "os casos cobrem tudo?", "caso de teste está correto?",
  "auditar suíte", "validar casos de teste". Also load when user pastes test cases
  and asks for feedback or improvements.
---

# Qualidade de Casos de Teste — Edge / UFAL

## Checklist obrigatório por caso de teste

**Cobertura:**

- [ ] Existe caso para o cenário feliz (fluxo principal com dados válidos)
- [ ] Existe caso para o cenário negativo (dados inválidos, erro, sem permissão)
- [ ] Edge cases foram considerados (vazio, limite, caractere especial)

**Gherkin:**

- [ ] Keywords em inglês (Given/When/Then/And/But)
- [ ] Restante em português
- [ ] Um único comportamento por cenário
- [ ] Steps atômicos
- [ ] Then é verificável de forma objetiva

**Nomenclatura:**

- [ ] Título no formato `[US-XXX] – Caso XX: [descrição]`
- [ ] Descrição é específica o suficiente para entender sem ler os steps

**Rastreabilidade:**

- [ ] Caso linkado ao critério de aceitação correspondente
- [ ] US de origem identificada no título

## Sinais de caso de teste fraco

- Then subjetivo: "o sistema deve funcionar corretamente"
- Step vago: "preencher o formulário com os dados"
- Múltiplos comportamentos no mesmo cenário
- Ausência de cenário negativo para funcionalidades críticas
```

---

### 3.5 acceptance-criteria

```bash
nano ~/.claude/skills/acceptance-criteria/SKILL.md
```

```markdown
---
name: acceptance-criteria
description: >
  Guidelines for reading and deriving test cases from acceptance criteria at Edge.
  Load this skill whenever the user is working with acceptance criteria — reading,
  writing, reviewing, or using them as a basis for test case generation. Trigger
  phrases: "critério de aceitação", "critérios de aceitação", "CA está bom?",
  "derivar casos do CA", "escrever CA", "revisar critério", "o CA cobre tudo?",
  "critério testável", "refinar critério de aceitação", "ler os CAs da US",
  "o que testar baseado no critério". Also load during refinement discussions.
---

# Critérios de Aceitação — Edge / UFAL

## O que é um bom CA

Um critério de aceitação deve ser:

- **Testável** — possível passar ou falhar de forma objetiva
- **Atômico** — um CA, um comportamento
- **Em Gherkin** — Given/When/Then (keywords em inglês)
- **Sem detalhes de implementação** — descreve o comportamento, não como é feito

## Checklist de avaliação

- [ ] Escrito em Gherkin com keywords em inglês?
- [ ] É possível derivar ao menos um caso de teste?
- [ ] Cobre apenas um comportamento?
- [ ] Claro o suficiente para um dev implementar sem dúvidas?

## Perguntas para fazer no refinamento

- "O que acontece se [campo obrigatório] estiver vazio?"
- "O que acontece se o usuário não tiver permissão?"
- "O que acontece se a operação falhar no servidor?"
- "Qual é o comportamento no limite máximo de caracteres/itens?"

## Derivando casos de teste de um CA

Para cada CA, identificar:

1. **Cenário feliz** — fluxo principal com dados válidos
2. **Cenário negativo** — dados inválidos ou condição de erro
3. **Edge case** — limite, vazio, valor máximo, caractere especial

**Exemplo:**
CA: `Given que o usuário está logado, When ele submete o formulário com dados
válidos, Then o documento deve ser salvo e exibido na listagem`

Deriva:

- `[US-XXX] – Caso 01: Salvar documento com todos os campos válidos`
- `[US-XXX] – Caso 02: Tentar salvar com campo obrigatório vazio`
- `[US-XXX] – Caso 03: Salvar com campo de texto no limite máximo de caracteres`
```

---

### 3.6 edge-qa-workflow

```bash
nano ~/.claude/skills/edge-qa-workflow/SKILL.md
```

```markdown
---
name: edge-qa-workflow
description: >
  Complete 6-step QA workflow at Edge, including Jira statuses, DoR, DoD, and
  tool responsibilities. Load this skill whenever the user asks about process,
  next steps, or what to do with a user story at any stage. Trigger phrases:
  "o que faço agora?", "próximo passo", "como proceder com a US?", "essa US
  está pronta para testar?", "posso fechar a US?", "o que é DoR?", "o que é
  DoD?", "quando movo para For Planning?", "quando movo para Closed?",
  "fluxo de trabalho", "processo de QA", "etapas do QA", "validar a US",
  "a US está pronta?". Also load when user mentions sprint planning or delivery.
---

# Fluxo de Trabalho do QA — Edge / UFAL

## As 6 Etapas

### Etapa 1 — Definição de US e Critérios de Aceitação

O QA participa do refinamento do backlog:

- Questionar ambiguidades na US
- Propor CAs em Gherkin (Given/When/Then)
- Colaborar com PM, UX e Devs

### Etapa 2 — Configuração no Qase e Jira

Após o refinamento:

- Criar suíte no Qase: `[EP1] > [US-XXX] > AC / TC`
- Registrar CAs na subseção `AC`
- Linkar suíte à US no Jira

### Etapa 3 — Definition of Ready (DoR)

Antes de aprovar a US para a sprint verificar:

- Descrição detalhada com regras de negócio ✓
- Protótipo de alta fidelidade validado pelo UX ✓
- Critérios de aceitação registrados no Qase ✓
  Se aprovada → mover US para **"For Planning"** no Jira

### Etapa 4 — Escrita de Casos de Teste

Enquanto os devs implementam:

- Criar TCs na subseção `TC` do Qase
- Cobrir cenário feliz, negativo e edge cases por critério
- Nomenclatura: `[US-XXX] – Caso XX: [descrição]`

### Etapa 5 — Validação e Relato de Bugs

Quando a US chega em **"Ready to Validation"**:

- Executar os TCs no Qase
- Reportar bugs com passos, evidências e severidade
- Bug bloqueante/alto → US retorna para **"For Planning"**

### Etapa 6 — Fechamento

Após correção de todos os bugs:

- Reexecutar os TCs
- Confirmar DoD (testes ok, documentação no Confluence)
- Mover US para **"Closed"** no Jira

## Status do Jira

| Status                  | Significa                                |
| ----------------------- | ---------------------------------------- |
| **For Planning**        | US aprovada na DoR, pronta para a sprint |
| **Ready to Validation** | Desenvolvimento concluído                |
| **Closed**              | US validada e entregue                   |
```

---

## 4. Commands

```bash
mkdir -p ~/.claude/commands/qa
```

---

### 4.1 /qa:test-from-us

```bash
nano ~/.claude/commands/qa/test-from-us.md
```

```markdown
Você vai gerar casos de teste completos para a User Story $ARGUMENTS.

## Pré-requisito obrigatório

Antes de qualquer ação, invoque as seguintes skills usando a ferramenta Skill:

1. `qase-structure` — hierarquia e nomenclatura obrigatórias do Qase
2. `gherkin-standards` — padrões Gherkin do time Edge
3. `acceptance-criteria` — diretrizes para derivar TCs a partir de critérios de aceitação

Toda criação de suítes, subseções e casos de teste deve seguir rigorosamente
essas regras.

## Passo a passo

1. Busque a issue $ARGUMENTS no Jira e leia:
   - Descrição completa da US (formato 3W: Como / Quero / Para que)
   - Critérios de aceitação registrados
   - Regras de negócio mencionadas

2. Busque a suíte correspondente no Qase e verifique se já existe estrutura
   criada para esta US. Se existir, leia os critérios já registrados em AC.

3. Para cada critério de aceitação, gere:
   - Caso feliz (fluxo principal com dados válidos)
   - Caso negativo (dados inválidos ou condição de erro)
   - Edge case (limite, vazio, valor máximo) quando aplicável

4. Para cada caso de teste, preencha obrigatoriamente:
   - **description**: objetivo do teste e critério de aceitação de referência
   - **preconditions**: estado necessário do sistema antes da execução
   - **postconditions**: estado esperado do sistema após a execução

5. Siga obrigatoriamente:
   - Keywords Gherkin em inglês: Given, When, Then, And, But
   - Restante da frase em português
   - Nomenclatura: [US-XXX] – Caso XX: [descrição curta]
   - Um comportamento por cenário

5. Apresente os casos gerados para revisão.

6. Após aprovação, pergunte se deve criar os casos na subseção TC
   da suíte correspondente no Qase. Só crie após confirmação explícita.
```

---

### 4.2 /qa:bug-analysis

```bash
nano ~/.claude/commands/qa/bug-analysis.md
```

```markdown
Você vai analisar o bug $ARGUMENTS em profundidade.

## Pré-requisito obrigatório

Antes de qualquer ação, invoque as seguintes skills usando a ferramenta Skill:

1. `bug-report` — template e padrões obrigatórios de reporte de bugs
2. `gherkin-standards` — padrões Gherkin do time Edge (para os casos de regressão)

## Passo a passo

1. Busque a issue $ARGUMENTS no Jira e leia todos os detalhes:
   - Descrição do problema
   - Passos para reproduzir (se houver)
   - Evidências anexadas
   - Comentários do time

2. Identifique e apresente:
   - Categoria: UI/Frontend, Backend/API, Banco de Dados, Integração,
     Performance ou Segurança
   - Módulo afetado no sistema
   - Severidade sugerida: Bloqueante / Alta / Média / Baixa
   - Possível causa raiz com base na descrição
   - Módulos relacionados que podem ser afetados

3. Verifique no Jira se existem bugs similares já reportados ou resolvidos.

4. Gere casos de teste de regressão em Gherkin para validar a correção:
   - Pelo menos um caso que reproduza o bug
   - Pelo menos um caso que valide o comportamento correto após correção
   - Keywords em inglês, restante em PT-BR

5. Pergunte se deve criar os casos de regressão no Qase.
   Só crie após confirmação explícita.
```

---

### 4.3 /qa:regression-check

```bash
nano ~/.claude/commands/qa/regression-check.md
```

```markdown
Você vai gerar um plano de regressão para $ARGUMENTS.

$ARGUMENTS pode ser: nome de um módulo, um épico ([EP1] Gestão de Documentos)
ou uma US específica ([US-XXX]).

## Pré-requisito obrigatório

Antes de qualquer ação, invoque a seguinte skill usando a ferramenta Skill:

1. `qase-structure` — hierarquia e nomenclatura obrigatórias do Qase

## Passo a passo

1. Busque no Qase todas as suítes relacionadas a $ARGUMENTS.

2. Para cada suíte encontrada, liste:
   - Total de casos de teste
   - Última execução (data e resultado)
   - Casos com histórico de falha

3. Classifique os casos por prioridade de regressão:
   - Crítico: cobre fluxo principal ou funcionalidade bloqueante
   - Importante: cobre funcionalidades de alta frequência de uso
   - Complementar: edge cases e cenários de baixo risco

4. Gere o plano de execução com:
   - Lista ordenada de casos por prioridade
   - Estimativa de tempo total de execução
   - Justificativa para os casos críticos

5. Pergunte se deve criar um ciclo de execução no Qase com estes casos.
   Só crie após confirmação explícita.
```

---

### 4.4 /qa:sprint-test-plan

```bash
nano ~/.claude/commands/qa/sprint-test-plan.md
```

```markdown
Você vai gerar o plano de testes para a sprint $ARGUMENTS.

## Pré-requisito obrigatório

Antes de qualquer ação, invoque as seguintes skills usando a ferramenta Skill:

1. `qase-structure` — hierarquia e nomenclatura obrigatórias do Qase
2. `acceptance-criteria` — diretrizes para mapear critérios de aceitação a casos de teste

## Passo a passo

1. Busque no Jira todas as US da sprint $ARGUMENTS com status
   diferente de "Closed".

2. Para cada US encontrada, verifique no Qase:
   - Se existe suíte criada
   - Se a subseção AC tem critérios registrados
   - Se a subseção TC tem casos de teste escritos
   - Quantos casos existem vs. quantos CAs foram mapeados

3. Classifique cada US:
   - ✅ Coberta — tem suíte, ACs e TCs criados
   - ⚠️ Parcial — tem suíte mas faltam TCs para algum critério
   - ❌ Sem cobertura — não tem suíte ou não tem TCs

4. Apresente o relatório de gaps com:
   - Lista de US sem cobertura ou cobertura parcial
   - Estimativa de esforço para cobrir os gaps
   - Sugestão de priorização baseada no status no Jira

5. Pergunte se deve criar as suítes e casos faltantes.
   Só crie após confirmação explícita.
```

---

### 4.5 /qa:doc-test-update

```bash
nano ~/.claude/commands/qa/doc-test-update.md
```

```markdown
Você vai atualizar a documentação de testes no Confluence para $ARGUMENTS.

$ARGUMENTS deve ser o identificador da US (ex: AIQA-1163 ou US-089).

## Pré-requisito obrigatório

Antes de qualquer ação, invoque a seguinte skill usando a ferramenta Skill:

1. `qase-structure` — hierarquia e nomenclatura obrigatórias do Qase (para navegar corretamente as suítes)

## Passo a passo

1. Busque no Qase a suíte correspondente a $ARGUMENTS e leia
   todos os casos de teste da subseção TC.

2. Busque no Jira a issue $ARGUMENTS e leia:
   - Título da US
   - Critérios de aceitação
   - Resultado da última validação (se disponível)

3. Busque no Confluence (espaço GD) a página de documentação da US.
   Se não existir, informe o usuário e pergunte se deve criar.

4. Monte o conteúdo de atualização com:
   - Status dos testes (quantos passaram, quantos falharam)
   - Lista de casos executados com resultado
   - Bugs encontrados (se houver)
   - Data da última execução

5. Apresente o conteúdo ao usuário para revisão.

6. Após aprovação, atualize a página no Confluence.
   Só atualize após confirmação explícita.
```

---

### 4.6 /qa:review-test-suite

```bash
nano ~/.claude/commands/qa/review-test-suite.md
```

```markdown
Você vai auditar a cobertura da suíte de testes para $ARGUMENTS.

$ARGUMENTS deve ser o identificador da US (ex: AIQA-1163 ou US-089).

## Pré-requisito obrigatório

Antes de qualquer ação, invoque as seguintes skills usando a ferramenta Skill:

1. `qase-structure` — hierarquia e nomenclatura obrigatórias do Qase
2. `gherkin-standards` — padrões Gherkin do time Edge (para os casos propostos)
3. `test-case-quality` — checklist de qualidade para auditoria de casos de teste
4. `acceptance-criteria` — diretrizes para mapear critérios de aceitação a casos de teste

## Passo a passo

1. Busque no Jira a issue $ARGUMENTS e leia todos os critérios
   de aceitação.

2. Busque no Qase a suíte de $ARGUMENTS e leia todos os casos
   existentes na subseção TC.

3. Para cada critério de aceitação, verifique:
   - Existe ao menos um caso cobrindo o cenário feliz?
   - Existe ao menos um caso cobrindo o cenário negativo?
   - Edge cases foram considerados?

4. Identifique os gaps:
   - Critérios sem nenhum caso de teste
   - Critérios com apenas cenário feliz (sem negativo)
   - Critérios com cobertura insuficiente de edge cases

5. Para cada gap, proponha os casos faltantes seguindo o padrão:
   - Keywords Gherkin em inglês, restante em PT-BR
   - Nomenclatura: [US-XXX] – Caso XX: [descrição]

6. Pergunte se deve criar os casos faltantes no Qase.
   Só crie após confirmação explícita.
```

---

### 4.7 /qa:close-us

```bash
nano ~/.claude/commands/qa/close-us.md
```

```markdown
Você vai executar o processo de fechamento da US $ARGUMENTS.

## Pré-requisito obrigatório

Antes de qualquer ação, invoque a seguinte skill usando a ferramenta Skill:

1. `edge-qa-workflow` — workflow completo do QA Edge, incluindo DoD e critérios de fechamento

## Passo a passo

1. Busque a issue $ARGUMENTS no Jira e verifique o status atual.

2. Verifique no Qase:
   - Todos os casos de teste da US têm status "passed"?
   - Existe algum caso com status "failed" ou "blocked"?

3. Verifique no Jira:
   - Existem bugs abertos vinculados a $ARGUMENTS?
   - Se sim, liste-os e informe o usuário — o fechamento não pode prosseguir
     enquanto houver bugs abertos.

4. Verifique o checklist da Definition of Done (DoD):
   - [ ] Todos os casos de teste passando no Qase
   - [ ] Nenhum bug aberto vinculado à US no Jira
   - [ ] Documentação atualizada no Confluence
   - [ ] Código revisado (PR aprovado)

5. Apresente o resultado do checklist ao usuário.

6. Se o checklist estiver completo, pergunte se deve mover a US para
   "Closed" no Jira. Só mova após confirmação explícita.

7. Após fechar, pergunte se deve registrar no Confluence a data de
   fechamento e o resumo do ciclo de testes.
```

---

## 5. Hooks

> **Pré-requisito:** `jq` deve estar instalado (`sudo apt install jq`).
> Os hooks do Claude Code recebem dados via stdin em JSON e devem retornar
> JSON válido no stdout para comunicar decisões de volta ao Claude.
> Mensagens para stderr só aparecem em modo verbose.
> Referência completa: https://code.claude.com/docs/en/hooks

### 5.1 Configuração — `~/.claude/settings.json`

Adicione o bloco `hooks` ao seu `settings.json` global. Observe que
`UserPromptSubmit` e `Stop` **não suportam matcher** — o campo deve ser
omitido. `PreToolUse` e `PostToolUse` usam matcher regex para filtrar
por nome da ferramenta.

```bash
nano ~/.claude/settings.json
```

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "bash $HOME/.claude/hooks/user-prompt-submit.sh"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "mcp__qase",
        "hooks": [
          {
            "type": "command",
            "command": "bash $HOME/.claude/hooks/pre-tool-call.sh"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "mcp__qase",
        "hooks": [
          {
            "type": "command",
            "command": "bash $HOME/.claude/hooks/post-tool-call.sh"
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "bash $HOME/.claude/hooks/stop.sh"
          }
        ]
      }
    ]
  }
}
```

---

### 5.2 Scripts dos Hooks

```bash
mkdir -p ~/.claude/hooks
```

---

#### user-prompt-submit.sh

Detecta keywords Gherkin em português no prompt e alerta sobre o padrão Edge
(keywords em inglês). O output é adicionado automaticamente como contexto.

```bash
nano ~/.claude/hooks/user-prompt-submit.sh
```

```bash
#!/bin/bash
# Hook: UserPromptSubmit
# Detecta keywords Gherkin em português e alerta sobre o padrão Edge

PROMPT=$(jq -r '.prompt // ""')

# Verifica se o prompt contém keywords Gherkin em português
FOUND=$(echo "$PROMPT" | grep -oiE '\b(Dado que|Quando|Então|Então que|E que|Mas que)\b' | head -1)

if [ -n "$FOUND" ]; then
  echo "⚠️ Detectado Gherkin em português no seu prompt (\"$FOUND\"). Lembre-se: keywords devem ser em inglês (Given, When, Then, And, But)."
else
  exit 0
fi
```

---

#### pre-tool-call.sh

Valida a nomenclatura de casos de teste antes de criar no Qase. Retorna
JSON com `hookSpecificOutput` contendo `permissionDecision` e
`additionalContext`.

```bash
nano ~/.claude/hooks/pre-tool-call.sh
```

```bash
#!/bin/bash
# Hook: PreToolUse
# Valida nomenclatura de casos de teste antes de criar no Qase

INPUT=$(cat)

TOOL_NAME=$(echo "$INPUT" | jq -r '.tool_name // ""')

if echo "$TOOL_NAME" | grep -qi "create_case"; then

  TITLE=$(echo "$INPUT" | jq -r '.tool_input.title // ""')

  if [ -n "$TITLE" ]; then
    if ! echo "$TITLE" | grep -qE '^\[US-[0-9]+\] – (Caso [0-9]+:|Critério [0-9]+:)'; then
      jq -n --arg title "$TITLE" '{
        hookSpecificOutput: {
          hookEventName: "PreToolUse",
          permissionDecision: "allow",
          additionalContext: ("⚠️ Atenção: o título \u0027" + $title + "\u0027 não segue o padrão obrigatório [US-XXX] – Caso XX: [descrição] ou [US-XXX] – Critério X: [descrição]. Verifique a nomenclatura.")
        }
      }'
    else
      jq -n --arg title "$TITLE" '{
        hookSpecificOutput: {
          hookEventName: "PreToolUse",
          permissionDecision: "allow",
          additionalContext: ("✅ Nomenclatura validada: " + $title)
        }
      }'
    fi
  else
    exit 0
  fi
else
  exit 0
fi
```

---

#### post-tool-call.sh

Registra um log local em `~/.claude/qa-metrics.log` quando casos de teste
são criados no Qase. Retorna JSON com `additionalContext` informando a
contagem do dia.

```bash
nano ~/.claude/hooks/post-tool-call.sh
```

```bash
#!/bin/bash
# Hook: PostToolUse
# Registra log local quando casos de teste são criados no Qase

INPUT=$(cat)

TOOL_NAME=$(echo "$INPUT" | jq -r '.tool_name // ""')

if echo "$TOOL_NAME" | grep -qi "create_case"; then

  LOG_FILE="$HOME/.claude/qa-metrics.log"
  DATE=$(date '+%Y-%m-%d')
  TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S')

  TITLE=$(echo "$INPUT" | jq -r '.tool_input.title // "sem título"')

  echo "$TIMESTAMP | ✅ CASO_CRIADO | $TITLE" >> "$LOG_FILE"

  COUNT=$(grep -c "$DATE" "$LOG_FILE" 2>/dev/null || echo "0")

  jq -n --arg count "$COUNT" '{
    hookSpecificOutput: {
      hookEventName: "PostToolUse",
      additionalContext: ("📊 Caso registrado no log de métricas. Total de casos criados via IA hoje: " + $count)
    }
  }'
else
  exit 0
fi
```

---

#### stop.sh

Salva blocos Gherkin gerados pelo Claude em arquivos `.feature` na pasta
`test-cases/generated/`. Verifica `stop_hook_active` para evitar loop
infinito.

```bash
nano ~/.claude/hooks/stop.sh
```

```bash
#!/bin/bash
# Hook: Stop
# Salva blocos Gherkin gerados em arquivos .feature na pasta test-cases/

INPUT=$(cat)

# Evitar loop infinito: se já estamos dentro de um stop hook, sair
STOP_ACTIVE=$(echo "$INPUT" | jq -r '.stop_hook_active // false')
if [ "$STOP_ACTIVE" = "true" ]; then
  exit 0
fi

RESPONSE=$(echo "$INPUT" | jq -r '.last_assistant_message // ""')

if echo "$RESPONSE" | grep -qE "(Given |When |Then |And |But )"; then

  OUTPUT_DIR="./test-cases/generated"
  mkdir -p "$OUTPUT_DIR"

  TIMESTAMP=$(date '+%Y%m%d_%H%M%S')
  US_ID=$(echo "$RESPONSE" | grep -oE '\[US-[0-9]+\]' | head -1 | tr -d '[]')
  US_ID=${US_ID:-desconhecida}
  OUTPUT_FILE="$OUTPUT_DIR/${US_ID}_${TIMESTAMP}.feature"

  echo "$RESPONSE" | python3 -c "
import sys, re
content = sys.stdin.read()
blocks = re.findall(r'\`\`\`gherkin\n(.*?)\`\`\`', content, re.DOTALL)
if blocks:
    print('\n\n'.join(blocks))
else:
    lines = content.split('\n')
    gherkin_lines = [l for l in lines if re.match(r'\s*(Feature:|Scenario:|Given |When |Then |And |But )', l)]
    if gherkin_lines:
        print('\n'.join(gherkin_lines))
" > "$OUTPUT_FILE"

  if [ -s "$OUTPUT_FILE" ]; then
    echo "💾 Gherkin salvo em: $OUTPUT_FILE" >&2
  else
    rm -f "$OUTPUT_FILE"
  fi
fi

exit 0
```

---

### 5.3 Tornar os scripts executáveis

```bash
chmod +x ~/.claude/hooks/*.sh
```

---

## 6. Verificação Final

Após criar todos os arquivos, execute:

```bash
claude
/qa:test-from-us AIQA-1163
```

**Resultado esperado:** Claude busca a US no Jira, lê os critérios de
aceitação e gera casos de teste com `Given/When/Then` em inglês e
restante em português, com nomenclatura `[US-XXX] – Caso XX:`.

---

_Setup do Ambiente · Módulo 1 · Curso IA para QA · Centro de Inovações Edge / UFAL_
