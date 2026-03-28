# Módulo 0 — Configuração do Ambiente

## Claude Code + Integrações MCP

**Curso:** IA Aplicada à Qualidade de Software  
**Instituição:** Centro de Inovações Edge — Universidade Federal de Alagoas  
**Formato:** Pré-curso · Assíncrono · Autoguiado  
**Estimativa de tempo:** ~2 horas

---

> ⚠️ **Este módulo não é ministrado em sala.** Você realiza a configuração no seu próprio ritmo, antes da primeira sessão presencial. Dúvidas? Fale com o professor pelos canais indicados no final deste documento. O **Módulo 1 não cobre instalações** — ele começa com o ambiente já pronto.

---

## O que você vai configurar

Ao concluir este módulo, você terá:

|  #  | O quê                     | Para quê                                                        |
| :-: | ------------------------- | --------------------------------------------------------------- |
|  1  | **Claude Code (CLI)**     | Usar IA diretamente no terminal, com acesso às suas ferramentas |
|  2  | **MCP Atlassian**         | Conectar o Claude ao Jira e ao Confluence da Edge               |
|  3  | **MCP Qase**              | Criar e gerenciar casos de teste via IA                         |
|  4  | **MCP Figma**             | Gerar Histórias de Usuário a partir de protótipos               |
|  5  | **Verificação integrada** | Confirmar que tudo funciona antes da primeira aula              |

---

## Pré-requisitos

Antes de começar, confirme que você possui:

- [ ] Conta ativa no **Claude.ai** (plano gratuito funciona para este módulo; plano Pro recomendado para os módulos seguintes)
- [ ] **Node.js 18 ou superior** instalado ([nodejs.org/pt/download](https://nodejs.org/pt/download))
- [ ] **Editor de código** instalado — VS Code ([code.visualstudio.com](https://code.visualstudio.com)) ou Cursor ([cursor.com](https://www.cursor.com)) — usado no Módulo 1
- [ ] Acesso ao **Jira e Confluence** da Edge (`kodercat.atlassian.net`)
- [ ] Acesso ao **Qase** — projeto `AIFORQAS` visível para você
- [ ] Acesso ao **Figma** — projeto de referência visível para você
- [ ] Sistema operacional compatível: macOS 10.15+, Ubuntu 20.04+/Debian 10+, ou Windows 10+ com WSL

> 💡 **Para verificar a versão do Node.js**, abra o terminal e execute: `node --version`  
> A saída deve ser algo como `v20.x.x` ou superior. Se for inferior a `v18`, atualize antes de continuar.

---

# Bloco 1 — Instalar o Claude Code

O Claude Code é uma ferramenta de terminal (CLI) que transforma o Claude em um assistente que age diretamente no seu ambiente de trabalho — lendo arquivos, executando comandos e se conectando às suas ferramentas via integrações MCP.

## 1.1 Instalação

Abra o terminal e execute o comando correspondente ao seu sistema operacional:

**macOS e Linux (incluindo WSL no Windows):**

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**Windows nativo (PowerShell):**

```powershell
irm https://claude.ai/install.ps1 | iex
```

> ⚠️ **Não use `sudo npm install -g`** — isso causa problemas de permissão. Use sempre os comandos acima.

Após a instalação, **feche e reabra o terminal** para que as variáveis de ambiente sejam atualizadas.

## 1.2 Verificar a instalação

```bash
claude --version
```

Você deve ver algo como:

```
Claude Code 1.x.x
```

Em seguida, execute o diagnóstico:

```bash
claude doctor
```

O comando verifica se a instalação está saudável e aponta possíveis problemas. Leia as saídas com atenção — se houver erros, compartilhe o resultado com o professor.

## 1.3 Autenticar no Claude

Na primeira vez que executar `claude`, você será solicitado a autenticar:

```bash
claude
```

Uma mensagem pedirá que você acesse um link no navegador para conectar sua conta do Claude.ai. Siga as instruções na tela e conclua o fluxo OAuth. Ao retornar ao terminal, você verá a tela inicial do Claude Code.

**Verificação rápida:** Digite `/help` no prompt do Claude e pressione Enter. Se aparecer o menu de ajuda, a autenticação foi bem-sucedida.

Para sair: `Ctrl + C` ou `/exit`.

---

# Bloco 2 — Configurar o MCP Atlassian (Jira + Confluence)

O MCP Atlassian conecta o Claude diretamente ao **Jira e ao Confluence** da Edge, permitindo consultar issues, criar tickets e gerar documentação sem sair do terminal.

## 2.1 Adicionar o servidor MCP

Execute este comando no terminal (fora do Claude Code, no shell normal):

```bash
claude mcp add --transport sse atlassian https://mcp.atlassian.com/v1/sse --scope user
```

> O parâmetro `--scope user` torna o servidor disponível em **todos os seus projetos**, não apenas no diretório atual. Isso é o que queremos para uso no dia a dia.

Você deve ver a mensagem: `MCP server added successfully`.

## 2.2 Autenticar no Atlassian

Inicie o Claude Code:

```bash
claude
```

Dentro do Claude Code, execute:

```
/mcp
```

Você verá `atlassian` listado. Se o status for `needs-auth`, o Claude solicitará que você abra um link no navegador para autorizar o acesso via OAuth. Siga o fluxo:

1. O link abre uma página da Atlassian no navegador
2. Selecione o workspace `kodercat` quando solicitado
3. Autorize as permissões (leitura e escrita em Jira e Confluence)
4. Volte ao terminal — a conexão é estabelecida automaticamente

## 2.3 Testar a conexão

Ainda dentro do Claude Code, envie esta mensagem:

```
Liste os projetos Jira que tenho acesso.
```

Se o Claude retornar uma lista com projetos (incluindo `AIQA`), a integração está funcionando. ✅

> ⚠️ **Reautenticação periódica:** O token do MCP Atlassian expira com frequência. Se o Claude disser que não consegue acessar o Jira, execute `/mcp` para verificar o status e siga o fluxo de autenticação novamente. Isso é esperado e não indica problema de configuração.

---

# Bloco 3 — Configurar o MCP Qase

O MCP Qase permite que o Claude crie, consulte e organize casos de teste diretamente no Qase — sem precisar abrir o navegador.

## 3.1 Gerar o token de API do Qase

Você precisará de um **token de API pessoal** do Qase para autenticar a integração.

1. Acesse [app.qase.io](https://app.qase.io) no navegador
2. Clique no seu avatar no canto superior direito → **API Tokens**
3. Clique em **+ Create API token**
4. Dê um nome descritivo, como `claude-code-edge`
5. Clique em **Create** e **copie o token gerado**

> 🔒 **Guarde este token em local seguro.** Ele só é exibido uma vez. Se perder, será necessário gerar um novo.

## 3.2 Adicionar o servidor MCP

No terminal, execute o comando abaixo **substituindo `SEU_TOKEN_AQUI` pelo token copiado**:

```bash
claude mcp add-json qase \
  '{"command":"npx","args":["-y","@qase/mcp-server"],"env":{"QASE_API_TOKEN":"SEU_TOKEN_AQUI"}}' \
  --scope user
```

**Exemplo com token fictício:**

```bash
claude mcp add-json qase \
  '{"command":"npx","args":["-y","@qase/mcp-server"],"env":{"QASE_API_TOKEN":"abc123xyz789"}}' \
  --scope user
```

> 💡 **Usuários de Windows (PowerShell):** Se o comando acima der erro com as aspas simples, use esta variante:
>
> ```powershell
> claude mcp add-json qase '{\"command\":\"npx\",\"args\":[\"-y\",\"@qase/mcp-server\"],\"env\":{\"QASE_API_TOKEN\":\"SEU_TOKEN_AQUI\"}}' --scope user
> ```

## 3.3 Testar a conexão

Inicie o Claude Code e envie:

```
Liste os projetos disponíveis no Qase.
```

O Claude deve retornar uma lista com seus projetos. Confirme que `AIFORQAS` aparece na lista. ✅

---

# Bloco 4 — Configurar o MCP Figma

O MCP Figma permite que o Claude leia protótipos do Figma para gerar Histórias de Usuário e Critérios de Aceitação diretamente a partir dos designs.

## 4.1 Adicionar o servidor MCP

No terminal, execute:

```bash
claude mcp add --transport http figma https://mcp.figma.com/mcp --scope user
```

> O MCP oficial do Figma usa transporte HTTP e autenticação via OAuth — não é necessário gerar token manualmente.

## 4.2 Autenticar no Figma

Inicie o Claude Code:

```bash
claude
```

Execute `/mcp`. Se `figma` aparecer com status `needs-auth`, o Claude exibirá um link. Abra-o no navegador:

1. Faça login na sua conta do Figma (se ainda não estiver logado)
2. Autorize a integração — conceda acesso de leitura aos seus projetos
3. Volte ao terminal — a conexão é estabelecida automaticamente

## 4.3 Testar a conexão

Dentro do Claude Code, envie:

```
Liste os projetos Figma que tenho acesso.
```

O Claude deve retornar seus projetos do Figma. Confirme que o projeto de referência da Edge aparece na lista. ✅

---

# Bloco 5 — Verificação Integrada

Com os quatro servidores configurados, vamos confirmar que tudo funciona junto antes da primeira sessão presencial.

## 5.1 Verificar o status de todos os MCPs

Inicie o Claude Code e execute:

```
/mcp
```

Você deve ver todos os servidores listados. O status ideal para cada um é `connected`:

```
MCP Server Status

• atlassian: connected
• qase:      connected
• figma:     connected
```

Se algum mostrar `failed` ou `needs-auth`, consulte a seção de solução de problemas ao final deste documento.

## 5.2 Prompt de verificação integrada

Cole este prompt no Claude Code para testar as quatro integrações de uma só vez:

```
Faça um diagnóstico rápido das minhas integrações:

1. Atlassian: liste o nome do projeto AIQA no Jira
2. Qase: confirme que o projeto AIFORQAS existe no Qase
3. Figma: liste o nome de um projeto que tenho acesso
4. Claude Code: mostre a versão atual instalada

Responda em formato de lista, indicando ✅ para cada integração que funcionou.
```

**Resultado esperado:**

```
✅ Atlassian (Jira): AIQA encontrado
✅ Qase: Projeto AIFORQAS confirmado (X casos de teste)
✅ Figma: [Nome do seu projeto] encontrado
✅ Claude Code: versão 1.x.x
```

Se todos os itens retornarem ✅, **você está pronto para o Módulo 1**. 🎉

---

# Bloco 6 — Checklist Final e Canais de Suporte

## Checklist de conclusão do Módulo 0

Marque cada item antes de comparecer à primeira sessão presencial:

**Instalação**

- [ ] Node.js 18+ instalado e verificado (`node --version`)
- [ ] Claude Code instalado sem erros (`claude --version`)
- [ ] `claude doctor` executado sem erros críticos
- [ ] Editor de código (VS Code ou Cursor) instalado e testado
- [ ] Autenticação no Claude.ai concluída

**Integrações MCP**

- [ ] MCP Atlassian adicionado (`--scope user`)
- [ ] Autenticação Atlassian via OAuth concluída
- [ ] Teste: projeto AIQA visível no Jira
- [ ] MCP Qase adicionado com token de API válido
- [ ] Teste: projeto AIFORQAS visível no Qase
- [ ] MCP Figma adicionado (`--scope user`)
- [ ] Autenticação Figma via OAuth concluída
- [ ] Teste: projetos Figma visíveis

**Verificação final**

- [ ] `/mcp` mostra todos os servidores como `connected`
- [ ] Prompt de verificação integrada retornou ✅ para todos os itens

---

## Canais de suporte

Encontrou algum problema? Fale com o professor pelos canais abaixo:

| Canal                   | Quando usar                                  | Como usar                                          |
| ----------------------- | -------------------------------------------- | -------------------------------------------------- |
| **Chat (mensagem)**     | Dúvidas rápidas de configuração              | Envie a mensagem de erro + sistema operacional     |
| **Videochamada (call)** | Problemas que precisam de tela compartilhada | Agende com o professor para um horário conveniente |
| **E-mail**              | Envio de capturas de tela ou logs de erro    | Descreva o passo em que travou e anexe o print     |

> 💡 **Dica:** ao pedir ajuda, inclua sempre: (1) o sistema operacional, (2) o passo em que o problema ocorreu, e (3) a mensagem de erro completa. Isso acelera muito a resolução.

---

# Apêndice — Solução de Problemas Comuns

## `claude: command not found` após instalação

O terminal não reconhece o comando. Solução:

```bash
# Recarregue as variáveis de ambiente do shell
source ~/.bashrc        # Linux/WSL com Bash
source ~/.zshrc         # macOS com Zsh

# Se o problema persistir, verifique onde o binário foi instalado
which claude
```

Se `which claude` não retornar nada, repita a instalação.

---

## `claude doctor` reporta erro de permissão

Nunca use `sudo` com o Claude Code. Se houver erros de permissão no npm:

```bash
# Verifique o dono do diretório global do npm
ls -la $(npm root -g)

# Se necessário, corrija as permissões
mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

---

## MCP Atlassian com status `failed`

1. Execute `/mcp` dentro do Claude Code para ver o erro detalhado
2. Remova e re-adicione o servidor:

```bash
claude mcp remove atlassian
claude mcp add --transport sse atlassian https://mcp.atlassian.com/v1/sse --scope user
```

3. Inicie o Claude Code e execute `/mcp` para refazer a autenticação OAuth

---

## MCP Qase: `Authentication failed`

O token está inválido ou expirou. Gere um novo token no Qase (Bloco 3.1) e atualize:

```bash
# Remover a configuração atual
claude mcp remove qase

# Re-adicionar com o novo token
claude mcp add-json qase \
  '{"command":"npx","args":["-y","@qase/mcp-server"],"env":{"QASE_API_TOKEN":"NOVO_TOKEN"}}' \
  --scope user
```

---

## MCP Figma não lista projetos

Verifique se você está logado na conta correta do Figma (a que tem acesso aos projetos da Edge). Se necessário, remova e refaça:

```bash
claude mcp remove figma
claude mcp add --transport http figma https://mcp.figma.com/mcp --scope user
```

Inicie o Claude Code, execute `/mcp` e refaça o fluxo OAuth com a conta certa.

---

## npx demora muito na primeira execução do Qase

Normal — o `npx` está baixando o pacote `@qase/mcp-server` pela primeira vez. As execuções seguintes são instantâneas pois o pacote fica em cache.

---

## Ver todos os servidores MCP configurados

```bash
# Listar todos os MCPs configurados (fora do Claude Code)
claude mcp list
```

---

## Onde ficam os arquivos de configuração?

| Sistema              | Localização                                |
| -------------------- | ------------------------------------------ |
| **macOS**            | `~/.claude.json` (configuração do usuário) |
| **Linux/WSL**        | `~/.claude.json`                           |
| **Windows (nativo)** | `%APPDATA%\Claude\claude.json`             |

Você pode abrir e inspecionar este arquivo para verificar as configurações dos MCPs. **Não compartilhe este arquivo** — ele pode conter tokens de API.

---

> **Meta do Módulo 0:** ao chegar na primeira sessão presencial, você deve ter o Claude Code instalado e as quatro integrações MCP operacionais. O Módulo 1 começa diretamente na prática — sem instalações, sem configurações.
>
> **Boa configuração, e até a primeira sessão!** 🚀
