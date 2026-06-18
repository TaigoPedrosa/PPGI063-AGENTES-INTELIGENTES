# Best Practices - Subagents

# Guideline: MCP e Sub-agentes em Agentes Inteligentes

## 1) Escopo e objetivo

Este guideline consolida diretrizes práticas sobre **Model Context Protocol (MCP)** e **sub-agentes** com base em:

1. na apresentação `PPGI_Agentes_Inteligentes___Claude_Code.pdf`;
2. em documentação oficial do MCP;
3. em documentação oficial do Claude Code;
4. em especificações técnicas citadas pelas próprias documentações.

Objetivo: oferecer um material de referência para desenho, operação e governança de sistemas agentivos com integração de ferramentas externas e execução paralela/isolada de tarefas.

---

## 2) MCP (Model Context Protocol)

## 2.1 Definição

O MCP é um **padrão aberto** para conectar aplicações de IA a fontes de dados e ferramentas externas, reduzindo integrações ponto a ponto entre cada agente e cada serviço.

- Na apresentação: MCP aparece como solução para o problema **N x M** de integrações (slide 19) e é descrito como padrão aberto com comunicação via **JSON-RPC 2.0** (slide 20).
- Na documentação oficial: o MCP é apresentado como padrão aberto para integração de aplicações LLM com dados, ferramentas e fluxos (`What is MCP?`).

## 2.2 Arquitetura conceitual (Host, Client, Server)

Modelo de papéis:

- **Host**: aplicação de IA que orquestra a interação com o usuário;
- **Client**: componente no host que mantém conexão com um servidor MCP;
- **Server**: processo local/remoto que expõe capacidades (ferramentas, recursos, prompts).

Esse modelo está na apresentação (slide 20) e na especificação do MCP (overview e arquitetura).

## 2.3 Primitivas centrais

O MCP organiza capacidades em três grupos (slide 21; especificação MCP):

- **Resources**: dados somente leitura (arquivos, registros, respostas de API);
- **Prompts**: templates reutilizáveis para interações e workflows;
- **Tools**: funções executáveis com potencial efeito colateral (chamar API, escrever em banco, executar comandos).

## 2.4 Transporte e protocolo base

- O protocolo base usa **JSON-RPC 2.0** (apresentação slide 20; especificação MCP).
- Transportes comuns citados: **STDIO** (processo local) e **HTTP/SSE** (remoto), com observação de que SSE aparece historicamente mas pode estar descontinuado em alguns clientes específicos (ver doc do Claude Code para comportamento atual).

## 2.5 Benefícios práticos esperados

1. **Interoperabilidade**: construir uma vez e integrar com múltiplos hosts compatíveis.
2. **Redução de acoplamento**: servidores de ferramenta desacoplados da aplicação de IA.
3. **Escalabilidade de integrações**: evita multiplicação de conectores proprietários.
4. **Padronização de capacidades**: recursos, prompts e tools com semântica comum.

## 2.6 Segurança e governança (diretrizes)

Com base na apresentação (slide 22) e na especificação MCP (seção de Security and Trust & Safety):

1. Exigir **consentimento explícito** do usuário para acesso a dados e execução de ações.
2. Tratar descrições de tools e conteúdo externo como **não confiáveis por padrão**.
3. Aplicar controles de autorização e princípio de menor privilégio por servidor/ferramenta.
4. Registrar auditoria de chamadas de tools e resultados críticos.
5. Em integrações OAuth, usar mecanismos de restrição de audiência/recurso quando aplicável (base conceitual alinhada ao RFC 8707).

---

## 3) Sub-agentes

## 3.1 Definição operacional

Sub-agente é uma instância separada de agente, criada pelo orquestrador para executar subtarefa específica em **contexto isolado**.

- Apresentação (slide 25): destaca paralelismo, isolamento de contexto, especialização e tolerância a falhas.
- Docs Claude Code: sub-agentes têm janela de contexto própria, podem ter prompt/scope de ferramentas próprios e retornam resumo ao fluxo principal.

## 3.2 Padrões de orquestração

### A) Orchestrator-Subagent (paralelo)

Fluxo típico (slide 27):

1. objetivo recebido pelo orquestrador;
2. decomposição em subtarefas independentes;
3. execução paralela;
4. coleta de resultados;
5. síntese/iteração.

Usar quando subtarefas são independentes e se beneficiam de concorrência.

### B) Pipeline sequencial

Fluxo típico (slide 27):

1. agente A gera artefato;
2. agente B valida;
3. agente C transforma/integra.

Usar quando há dependência forte entre etapas e necessidade de gates de qualidade.

## 3.3 Isolamento e integração de resultados

Diretrizes:

1. Definir contrato claro de entrada/saída para cada sub-agente.
2. Evitar compartilhamento implícito de estado; passar dados explicitamente.
3. Consolidar retorno em formato padronizado (achados, evidências, próximos passos).
4. Quando disponível, usar isolamento por worktree para sandbox de alterações.

## 3.4 Limitações e riscos

Conforme apresentação (slide 28) e docs oficiais:

- **Custo/token** cresce com número de sub-agentes;
- **Sem estado compartilhado automático**;
- **Falhas locais** exigem recuperação pelo orquestrador;
- **Risco de propagação de prompt injection** entre artefatos/resultados;
- **Latência operacional** pode aumentar em cadeias longas.

Mitigações recomendadas:

1. validar saída de sub-agente antes de promover para contexto principal;
2. limitar escopo de tools por papel;
3. padronizar política de retry, timeout e fallback;
4. usar sub-agentes apenas quando houver ganho real de isolamento ou paralelismo.

---

## 4) Guia de adoção (checklist)

## 4.1 Para MCP

- [ ]  Mapear ferramentas/dados externos prioritários.
- [ ]  Definir servidores MCP por domínio (ex.: design, SCM, banco, observabilidade).
- [ ]  Classificar cada tool por risco e nível de permissão.
- [ ]  Implementar política de consentimento, logs e revisão de segurança.
- [ ]  Medir impacto de contexto e ajustar estratégia de carregamento de ferramentas.

## 4.2 Para sub-agentes

- [ ]  Definir catálogo de papéis (ex.: pesquisa, implementação, teste, revisão).
- [ ]  Estabelecer template de prompt e formato de saída por papel.
- [ ]  Escolher padrão de execução (paralelo vs pipeline) por tipo de tarefa.
- [ ]  Adotar critérios de “quando não usar sub-agente”.
- [ ]  Instrumentar métricas de custo, latência e taxa de retrabalho.

---

## 5) Quando usar MCP e sub-agentes juntos

Use combinação MCP + sub-agentes quando:

1. houver múltiplas integrações externas a consultar em paralelo;
2. cada subtarefa demandar conjunto distinto de tools/permissões;
3. o volume de saída puder saturar o contexto da thread principal;
4. for importante separar pesquisa/extração de dados da etapa de síntese final.

Exemplo de desenho:

- Sub-agente A: coleta requisitos em issue tracker (MCP).
- Sub-agente B: consulta dados operacionais/erros (MCP).
- Sub-agente C: avalia impacto em código e testes.
- Orquestrador: consolida, prioriza e propõe plano executável.

---

## 6) Referências

### 6.1 Fonte primária da apresentação

1. Wagner, D. L. P.; Taígo; João Victor. **Intelligent Agents: A Study with Claude Code (Netflix Clone Use Case)**. PPGI, 2026. Arquivo: `PPGI_Agentes_Inteligentes___Claude_Code.pdf`.
    - Tópicos usados: MCP (slides 18-23) e Sub-agents (slides 24-28).

### 6.2 Documentação oficial

1. Model Context Protocol. **What is MCP?** Disponível em: `https://modelcontextprotocol.io/introduction`.
2. Model Context Protocol. **Specification (2025-03-26)**. Disponível em: `https://modelcontextprotocol.io/specification/2025-03-26`.
3. Anthropic (Claude Code Docs). **Connect Claude Code to tools via MCP**. Disponível em: `https://docs.anthropic.com/en/docs/claude-code/mcp`.
4. Anthropic (Claude Code Docs). **Create custom subagents**. Disponível em: `https://docs.anthropic.com/en/docs/claude-code/sub-agents`.

### 6.3 Especificações técnicas relacionadas

1. JSON-RPC Working Group. **JSON-RPC 2.0 Specification**. Disponível em: `https://www.jsonrpc.org/specification`.
2. Campbell, B.; Bradley, J.; Tschofenig, H. **RFC 8707 - Resource Indicators for OAuth 2.0**. IETF, 2020. Disponível em: `https://www.rfc-editor.org/rfc/rfc8707`.

---

## 7) Nota metodológica

Este documento evita afirmações não rastreáveis. Quando um detalhe depende de implementação específica (por exemplo, comportamento de transporte, carregamento de tools ou regras de execução em background), a referência prioritária é a documentação oficial do produto (Claude Code docs) e, para semântica de protocolo, a especificação oficial do MCP.
