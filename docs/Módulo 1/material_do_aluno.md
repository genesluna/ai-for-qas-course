# Módulo 1 — Material do Aluno

## Fundamentos e Ferramentas do Claude Code para QA

**Curso:** IA para Analistas de QA · Centro de Inovações Edge / UFAL  
**Duração:** 2 horas

---

## O que você vai aprender

1. O que é o Claude Code e como ele difere do Claude.ai
2. Como configurar o `CLAUDE.md` global com os padrões do Edge
3. O que são Skills e como elas funcionam automaticamente
4. Como criar e invocar Commands customizados
5. O que são Hooks e como automatizam o seu fluxo
6. O conceito de Sub-agents

---

## 1. O que é o Claude Code

O Claude Code é uma ferramenta de linha de comando que permite trabalhar com
IA diretamente no terminal, com acesso ao Jira, Qase, Confluence e arquivos
do projeto.

|                     | Claude Code (desktop)                 | Claude Code (terminal)              |
| ------------------- | ------------------------------------- | ----------------------------------- |
| Interface           | Gráfica — cliques, drag & drop        | Linha de comando                    |
| Skills e Commands   | Disponíveis via botão `/`             | Disponíveis via `/nome`             |
| Hooks               | Não suportados                        | ✅ Suportados                       |
| Automação e scripts | Limitada                              | ✅ Total                            |
| Anexar evidências   | Arrastar imagens/PDFs direto na UI    | Via caminho do arquivo              |
| Melhor para         | Uso do dia a dia com interface visual | Automação, hooks e fluxos avançados |

```bash
claude --version      # verificar instalação
cd /pasta/do/projeto  # entrar no projeto
claude                # iniciar
```

---

## 2. CLAUDE.md Global

O arquivo `~/.claude/CLAUDE.md` carrega automaticamente em **todos os projetos**
no seu computador. É onde ficam os padrões do Edge — não interfere no
`CLAUDE.md` dos devs.

**O que entra aqui:** padrões de Gherkin, nomenclatura, severidade de bugs.  
**O que não entra:** nome do projeto, URL do Jira, ID do Qase.

```bash
nano ~/.claude/CLAUDE.md
```

---

## 3. Skills

Skills são arquivos `SKILL.md` que ensinam o Claude a aplicar conhecimento
especializado. Ficam em `~/.claude/skills/` e carregam automaticamente
quando o Claude julga que são relevantes para o que você está fazendo.

### Estrutura de uma Skill

```
~/.claude/skills/
└── nome-da-skill/
    └── SKILL.md
```

```markdown
---
name: nome-da-skill
description: Quando esta skill deve ser carregada automaticamente.
---

[instruções para o Claude]
```

### As 6 Skills do QA Edge

| Skill                 | Quando ativa automaticamente                   |
| --------------------- | ---------------------------------------------- |
| `gherkin-standards`   | Ao escrever ou revisar cenários Gherkin        |
| `qase-structure`      | Ao criar ou organizar suítes no Qase           |
| `bug-report`          | Ao reportar ou analisar bugs                   |
| `test-case-quality`   | Ao revisar ou avaliar casos de teste           |
| `acceptance-criteria` | Ao trabalhar com critérios de aceitação        |
| `edge-qa-workflow`    | Ao perguntar sobre próximos passos ou processo |

### Como instalar

```bash
mkdir -p ~/.claude/skills/gherkin-standards
# Criar o SKILL.md com o conteúdo fornecido pelo ministrante
nano ~/.claude/skills/gherkin-standards/SKILL.md
# Repetir para cada skill
```

---

## 4. Commands

Commands são arquivos `.md` em `~/.claude/commands/` invocados com `/nome`.
Cada um define um workflow que o Claude executa ao ser chamado.
`$ARGUMENTS` captura o que você digita após o `/nome`.

```
~/.claude/
└── commands/
    └── meu-command.md    → invocado com /meu-command [argumentos]
```

### Os 7 Commands do QA Edge

| Command                 | Como usar                         | O que faz                                    |
| ----------------------- | --------------------------------- | -------------------------------------------- |
| `/qa:test-from-us`      | `/qa:test-from-us AIQA-1163`      | US → casos de teste em Gherkin → Qase        |
| `/qa:bug-analysis`      | `/qa:bug-analysis AIQA-1164`      | Bug → categorização + causa raiz + regressão |
| `/qa:regression-check`  | `/qa:regression-check "módulo X"` | Módulo → plano de regressão priorizado       |
| `/qa:sprint-test-plan`  | `/qa:sprint-test-plan Sprint-42`  | Sprint → gaps de cobertura                   |
| `/qa:doc-test-update`   | `/qa:doc-test-update AIQA-1163`   | Suite Qase → documentação Confluence         |
| `/qa:review-test-suite` | `/qa:review-test-suite AIQA-1163` | Suite existente → gaps de cobertura          |
| `/qa:close-us`          | `/qa:close-us AIQA-1163`          | Valida DoD → fecha a US no Jira              |

### Como criar um Command

```bash
mkdir -p ~/.claude/commands/qa
nano ~/.claude/commands/qa/test-from-us.md
# Colar o conteúdo fornecido pelo ministrante
```

---

## 5. Hooks

Hooks são scripts executados automaticamente em pontos específicos
do ciclo de trabalho. A configuração fica em `~/.claude/settings.json`.

### Os 4 Hooks do QA Edge

| Hook               | Quando executa                   | Para que serve                             |
| ------------------ | -------------------------------- | ------------------------------------------ |
| `UserPromptSubmit` | Antes de enviar cada mensagem    | Detecta Gherkin em português no prompt     |
| `PreToolUse`       | Antes de chamar Jira, Qase, etc. | Valida nomenclatura antes de criar no Qase |
| `PostToolUse`      | Após ferramenta retornar         | Registra métricas locais de criação        |
| `Stop`             | Ao Claude terminar uma resposta  | Salva Gherkin gerado em `.feature`         |

### Conceito — só como referência para leitura

> O código completo dos hooks está no material do ministrante.
> Neste módulo você vai entender o que cada hook faz.
> A implementação prática acontece no exercício guiado.

---

## 6. Sub-agents (conceito)

Sub-agents são instâncias auxiliares que o Claude Code cria para executar
tarefas em paralelo. O agente principal delega e recebe apenas o resumo.

**Quando são úteis para QA:**

- Analisar cobertura de vários módulos ao mesmo tempo
- Gerar casos de teste em lote para uma release
- Verificar múltiplas US da sprint em paralelo

**Como ativar:**

```
"Use sub-agents paralelos para analisar a cobertura de cada módulo
da sprint. Módulos: Documentos, Aprovações, Usuários.
Consolide os resultados ao final."
```

---

## Exercício — Setup Completo

> 📄 Use o arquivo `Modulo1-Setup-do-Ambiente.md` durante este exercício.
> Todo o código já está pronto — basta abrir o arquivo, copiar e colar
> em cada arquivo indicado abaixo.

### Passo 1 — CLAUDE.md do Projeto (2 min)

Abra a **Seção 1** do `Modulo1-Setup-do-Ambiente.md`:

- **Caso A:** o projeto já tem `CLAUDE.md` → acrescente a seção QA ao final
- **Caso B:** o projeto não tem `CLAUDE.md` → crie do zero com o template

> ⚠️ **Não commitar** este arquivo com as suas alterações de QA.

### Passo 2 — CLAUDE.md global (2 min)

Abra a **Seção 2** do `Modulo1-Setup-do-Ambiente.md`:

```bash
nano ~/.claude/CLAUDE.md
```

### Passo 3 — Instalar as Skills (3 min)

Abra a **Seção 3** do `Modulo1-Setup-do-Ambiente.md`:

```bash
mkdir -p ~/.claude/skills/{gherkin-standards,qase-structure,bug-report,\
test-case-quality,acceptance-criteria,edge-qa-workflow}
```

Crie cada `SKILL.md` copiando o conteúdo da subseção correspondente.

### Passo 4 — Criar os Commands (3 min)

Abra a **Seção 4** do `Modulo1-Setup-do-Ambiente.md`:

```bash
mkdir -p ~/.claude/commands/qa
```

Crie cada `.md` copiando o conteúdo da subseção correspondente.

### Passo 5 — Testar (2 min)

```bash
claude
/qa:test-from-us AIQA-1163
```

**Resultado esperado:** Claude gera casos de teste com `Given/When/Then`
em inglês, restante em português, nomenclatura `[US-XXX] – Caso XX:`.

### Checklist de conclusão

- [ ] `CLAUDE.md` do projeto atualizado com a seção QA (Seção 1 do setup)
- [ ] `~/.claude/CLAUDE.md` global criado (Seção 2 do setup)
- [ ] 6 Skills instaladas em `~/.claude/skills/` (Seção 3 do setup)
- [ ] 7 Commands criados em `~/.claude/commands/qa/` (Seção 4 do setup)
- [ ] Hooks configurados pelo ministrante (Seção 5 do setup)
- [ ] `/qa:test-from-us` executado com sucesso em uma US real

---

## O fluxo de trabalho e onde a IA entra

| Etapa                          | Você faz                                       | IA ajuda                               |
| ------------------------------ | ---------------------------------------------- | -------------------------------------- |
| **1 — Definição de US e CAs**  | Refinamento presencial                         | _(a IA não substitui o refinamento)_   |
| **2 — Configuração Qase/Jira** | Criar suíte e registrar CAs                    | `/qa:test-from-us` cria a estrutura    |
| **3 — DoR**                    | Verificar se a US está pronta → "For Planning" | `/qa:sprint-test-plan` identifica gaps |
| **4 — Casos de Teste**         | Revisar e ajustar o que a IA gerou             | `/qa:test-from-us` gera os TCs         |
| **5 — Validação e Bugs**       | Executar TCs e reportar bugs                   | `/qa:bug-analysis` analisa os bugs     |
| **6 — Fechamento**             | Confirmar DoD e fechar                         | `/qa:close-us` executa o checklist     |

---

## Padrões do Edge — Referência Rápida

### Gherkin

```gherkin
Given que estou logado como analista de qualidade
And estou na tela de listagem de documentos
When eu aplico o filtro por tipo "Contrato"
Then apenas documentos do tipo "Contrato" devem ser exibidos
```

- Keywords **em inglês**: `Given`, `When`, `Then`, `And`, `But`
- Restante **em português**
- Um único comportamento por cenário

### Nomenclatura de casos de teste

```
[US-089] – Caso 01: Listar documentos com filtro ativo
[US-089] – Caso 02: Listar documentos sem permissão de visualização
```

### Estrutura do Qase

```
Suíte: [EP1] Nome do Épico
└── [US-XXX] – Nome da US
    ├── AC – Critérios de Aceitação
    │   └── [US-XXX] – Critério 1: [descrição]
    └── TC – Casos de Teste
        └── [US-XXX] – Caso 01: [descrição]
```

### Severidade de bugs

| Severidade     | Quando usar                                     |
| -------------- | ----------------------------------------------- |
| **Bloqueante** | Fluxo principal impossível de executar          |
| **Alta**       | Funcionalidade crítica com comportamento errado |
| **Média**      | Bug com contorno disponível                     |
| **Baixa**      | Cosmético / UX não crítico                      |

---

## Estrutura de Arquivos

```
~/.claude/
├── CLAUDE.md                         ← padrões globais do Edge
├── settings.json                     ← configuração dos hooks
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
```

---

## Tarefa para o Módulo 2

1. Usar `/qa:test-from-us` em **pelo menos 2 US reais** do seu backlog atual
2. Verificar se os casos gerados seguem os padrões Gherkin do Edge
3. Anotar: o que a IA acertou? O que precisou de ajuste?

---

_Módulo 1 · Material do Aluno · Curso IA para QA · Centro de Inovações Edge / UFAL_
