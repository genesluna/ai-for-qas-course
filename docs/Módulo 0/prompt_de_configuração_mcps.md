# Prompt de Configuração Automática — Módulo 0

## Cole este conteúdo diretamente no Claude Code

---

Você é um assistente de configuração de ambiente. Sua tarefa é instalar e verificar as integrações MCP necessárias para o curso "IA Aplicada à Qualidade de Software" do Centro de Inovações Edge — UFAL.

Siga **rigorosamente** a sequência abaixo. A cada etapa:

- Execute o comando indicado via Bash
- Reporte o resultado antes de prosseguir
- Se houver erro, informe o problema com clareza e aguarde instrução

**Não pule etapas. Não faça suposições silenciosas.**

---

## ETAPA 1 — Verificação do ambiente

Execute os comandos abaixo e reporte os resultados:

```bash
node --version
claude mcp list
```

**Node.js:**

- Se retornar versão inferior a `v18`, **pare** e informe: "Node.js desatualizado. Acesse nodejs.org/pt/download, instale a versão LTS e reinicie o Claude Code antes de continuar."
- Se retornar `v18` ou superior, confirme e prossiga.

**MCPs já instalados (`claude mcp list`):**

- Anote quais dos três servidores — `atlassian`, `qase`, `figma` — já aparecem na lista.
- Para cada um que **já estiver instalado**, a etapa correspondente será pulada.
- Informe ao usuário quais MCPs já estão presentes e quais serão instalados agora.

---

## ETAPA 2 — MCP Atlassian (Jira + Confluence)

> **Verificação prévia:** se `atlassian` já apareceu na lista da Etapa 1, **pule esta etapa** e informe ao usuário: "MCP Atlassian já instalado. Pulando para a Etapa 3."

Caso não esteja instalado, execute:

```bash
claude mcp add --transport sse atlassian https://mcp.atlassian.com/v1/sse --scope user
```

Após o comando:

1. Confirme se a saída contém "MCP server added successfully"
2. Informe ao usuário:

> ⚠️ **Ação necessária — autenticação Atlassian:**
> Execute `/mcp` nesta sessão do Claude Code. Se o status de `atlassian` for `needs-auth`, um link será exibido. Abra-o no navegador, selecione o workspace `kodercat` e autorize o acesso. Quando concluir, responda **"atlassian ok"** para continuar.

Aguarde a confirmação do usuário antes de prosseguir.

---

## ETAPA 3 — MCP Qase

> **Verificação prévia:** se `qase` já apareceu na lista da Etapa 1, **pule esta etapa** e informe ao usuário: "MCP Qase já instalado. Pulando para a Etapa 4."

Caso não esteja instalado, siga:

> ⚠️ **Ação necessária — token de API:**
> Para instalar o MCP do Qase, você precisa de um token de API pessoal.
>
> **Como obter:**
>
> 1. Acesse [app.qase.io](https://app.qase.io) no navegador
> 2. Clique no seu avatar (canto superior direito) → **API Tokens**
> 3. Clique em **+ Create API token**, dê o nome `claude-code-edge` e clique em **Create**
> 4. **Copie o token** (ele só é exibido uma vez)
>
> Quando tiver o token, responda neste chat com:
> `token qase: SEU_TOKEN_AQUI`
>
> ⚠️ **Atenção:** o token ficará registrado no histórico desta conversa. Se preferir, você pode executar o comando de instalação diretamente no terminal (fora do Claude Code) — veja o Bloco 3.2 do guia de configuração.

Aguarde o usuário fornecer o token. Quando receber, execute o comando abaixo **substituindo `TOKEN_RECEBIDO` pelo valor informado**:

```bash
claude mcp add-json qase \
  '{"command":"npx","args":["-y","@qase/mcp-server"],"env":{"QASE_API_TOKEN":"TOKEN_RECEBIDO"}}' \
  --scope user
```

> 💡 **Usuários de Windows (PowerShell):** se o comando acima falhar por conta das aspas, use esta variante:
>
> ```powershell
> claude mcp add-json qase '{\"command\":\"npx\",\"args\":[\"-y\",\"@qase/mcp-server\"],\"env\":{\"QASE_API_TOKEN\":\"TOKEN_RECEBIDO\"}}' --scope user
> ```

Confirme o sucesso e prossiga para a Etapa 4.

---

## ETAPA 4 — MCP Figma

> **Verificação prévia:** se `figma` já apareceu na lista da Etapa 1, **pule esta etapa** e informe ao usuário: "MCP Figma já instalado. Pulando para a Etapa 5."

Caso não esteja instalado, execute:

```bash
claude mcp add --transport http figma https://mcp.figma.com/mcp --scope user
```

Após o comando:

1. Confirme se a saída contém sucesso
2. Informe ao usuário:

> ⚠️ **Ação necessária — autenticação Figma:**
> Execute `/mcp` nesta sessão. Se o status de `figma` for `needs-auth`, um link será exibido. Abra-o no navegador, faça login com a conta que tem acesso aos projetos da Edge e autorize. Quando concluir, responda **"figma ok"** para continuar.

Aguarde a confirmação do usuário antes de prosseguir.

---

## ETAPA 5 — Verificação final

Execute e reporte o resultado:

```bash
claude mcp list
```

Confirme que `atlassian`, `qase` e `figma` aparecem na lista.

Em seguida, teste cada integração executando as seguintes consultas via MCP:

1. **Atlassian:** busque o projeto `AIQA` no Jira
2. **Qase:** liste os projetos disponíveis e confirme que `AIFORQAS` existe
3. **Figma:** liste os projetos Figma acessíveis ao usuário

> 💡 Execute essas consultas você mesmo usando as ferramentas MCP — não peça ao usuário para testá-las separadamente.

Com base nos resultados, gere um relatório de configuração no seguinte formato:

---

### ✅ Relatório de Configuração — Módulo 0

| Integração    | Status | Observação                          |
| ------------- | :----: | ----------------------------------- |
| Node.js       | ✅/❌  | versão X.X.X                        |
| MCP Atlassian | ✅/❌  | projetos visíveis / erro encontrado |
| MCP Qase      | ✅/❌  | projetos visíveis / erro encontrado |
| MCP Figma     | ✅/❌  | projetos visíveis / erro encontrado |

**Ambiente pronto para o Módulo 1:** SIM / NÃO

Se houver algum item com ❌, descreva o problema e a ação recomendada.

---

Configuração concluída. Você pode fechar esta conversa e aguardar a primeira sessão presencial.
