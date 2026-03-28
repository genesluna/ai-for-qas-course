# IA Aplicada à Qualidade de Software

Uso de Inteligência Artificial no Fluxo de Trabalho do Analista de QA

**Centro de Inovações Edge — Universidade Federal de Alagoas**

| Modalidade | Carga Horária  | Módulos              | Público         |
| ---------- | -------------- | -------------------- | --------------- |
| Presencial | 12 horas-aula  | M0 + 6 módulos × 2h | Analistas de QA |

---

## Sobre o Curso

O curso integra IA generativa ao fluxo de trabalho do analista de QA, cobrindo o ciclo completo: escrita de Histórias de Usuário, geração de casos de teste em Gherkin/BDD, análise de bugs e documentação técnica — tudo conectado às ferramentas já usadas pelo time (Jira, Qase, Confluence e Figma).

A IA é posicionada como ferramenta de amplificação da expertise existente: ela reduz esforço em tarefas repetitivas e libera o analista para decisões que exigem experiência de domínio e senso crítico.

---

## Estrutura dos Módulos

| Módulo | Título                                        | Formato                |
| :----: | --------------------------------------------- | ---------------------- |
| **M0** | Configuração do Ambiente (Claude Code + MCPs) | Assíncrono (~2h)       |
|   M1   | Fundamentos (Skills, Commands, Agents, Hooks) | Expositiva + Prática   |
|   M2   | Geração de Histórias de Usuário com IA        | Demo + Laboratório     |
|   M3   | Geração de Casos de Teste com IA e Qase       | Demo + Laboratório     |
|   M4   | Análise e Categorização de Bugs               | Demo + Laboratório     |
|   M5   | Documentação com IA no Confluence             | Demo + Laboratório     |
|   M6   | Prática Integradora e Avaliação Final         | Projeto + Avaliação    |

O **Módulo 0** é pré-curso e assíncrono — o aluno configura o ambiente antes da primeira sessão presencial. Os módulos 1 a 6 são presenciais, com 2 horas-aula cada.

---

## Pré-requisitos

- Conta ativa no [Claude.ai](https://claude.ai) (plano Pro recomendado)
- [Node.js 18+](https://nodejs.org/pt/download)
- Editor de código — [VS Code](https://code.visualstudio.com) ou [Cursor](https://www.cursor.com)
- Acesso ao Jira e Confluence da Edge (`kodercat.atlassian.net`)
- Acesso ao [Qase](https://app.qase.io) — projeto `AIFORQAS`
- Acesso ao [Figma](https://www.figma.com) — projeto de referência do curso
- macOS 10.15+, Ubuntu 20.04+/Debian 10+, ou Windows 10+ com WSL

---

## Configuração do Ambiente

Consulte o [guia de configuração do Módulo 0](docs/Módulo%200/configuração_do_ambiente.md) para instruções detalhadas de instalação do Claude Code e das integrações MCP.

Para configuração assistida, cole o conteúdo do [prompt de configuração automática](docs/Módulo%200/prompt_de_configuração_mcps.md) diretamente no Claude Code.

---

## Ferramentas Utilizadas

| Ferramenta         | Uso no Curso                                              |
| ------------------ | --------------------------------------------------------- |
| **Claude Code**    | IA no terminal — geração, análise e automação via MCP     |
| **Jira**           | Histórias de Usuário e relatórios de bug                  |
| **Qase**           | Casos de teste em Gherkin/BDD                             |
| **Figma**          | Protótipos como fonte para geração de US                  |
| **Confluence**     | Documentação técnica de QA                                |

---

## Estrutura do Repositório

```
docs/
├── ementa.md                           # Ementa completa do curso
├── fluxo_de_trabalho_qas_edge.md       # Fluxo de trabalho QA da Edge
├── Módulo 0/
│   ├── configuração_do_ambiente.md     # Guia de instalação e configuração
│   └── prompt_de_configuração_mcps.md  # Prompt para configuração assistida
└── Módulo 1/
    ├── material_do_aluno.md            # Material do aluno — Sessão 1
    ├── material_do_professor.md        # Material do professor — Sessão 1
    └── configuração_do_ambiente.md     # Configuração específica do Módulo 1
```

---

## Licença

Consulte o arquivo [LICENSE](LICENSE) para detalhes.
