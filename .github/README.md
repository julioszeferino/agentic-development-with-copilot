# Claude to Copilot - Agent Hub

Este diretorio centraliza agentes, prompts, Knowledge Base, Dev Loop e SDD para desenvolvimento agentico com GitHub Copilot.

## Visao Geral

- `agents/`: especialistas por dominio e por fase de workflow.
- `prompts/`: comandos de alto nivel (`/dev`, `/workflow_*`, etc.).
- `kb/`: base de conhecimento modular para suporte tecnico.
- `dev/`: Dev Agentic Loop (iteracao estruturada e recuperacao de sessao).
- `sdd/`: Spec-Driven Development em 5 fases.

## Catalogo de Agentes

Total de agentes: **31** agentes especializados + **1 template**.

### AI/ML (`agents/ai-ml`)

| Agente | Arquivo | Foco Principal |
|---|---|---|
| `ai-data-engineer` | `agents/ai-ml/ai-data-engineer.agent.md` | Arquitetura de dados, pipelines AI e cloud |
| `ai-prompt-specialist` | `agents/ai-ml/ai-prompt-specialist.agent.md` | Otimizacao de prompts e output estruturado |
| `genai-architect` | `agents/ai-ml/genai-architect.agent.md` | Arquitetura GenAI e orquestracao multiagente |
| `llm-specialist` | `agents/ai-ml/llm-specialist.agent.md` | Engenharia de prompts e extracao com LLM |

### Code Quality (`agents/code-quality`)

| Agente | Arquivo | Foco Principal |
|---|---|---|
| `code-cleaner` | `agents/code-quality/code-cleaner.agent.md` | Refactor e limpeza de codigo Python |
| `code-documenter` | `agents/code-quality/code-documenter.agent.md` | Documentacao tecnica e README |
| `code-reviewer` | `agents/code-quality/code-reviewer.agent.md` | Review de qualidade, seguranca e manutencao |
| `python-developer` | `agents/code-quality/python-developer.agent.md` | Desenvolvimento Python para pipelines/parsers |
| `shell-script-specialist` | `agents/code-quality/shell-script-specialist.agent.md` | Scripts Bash de automacao/deploy/testes |

### Communication (`agents/communication`)

| Agente | Arquivo | Foco Principal |
|---|---|---|
| `meeting-analyst` | `agents/communication/meeting-analyst.agent.md` | Sintese de reunioes em documentacao acionavel |
| `notes-analyst` | `agents/communication/notes-analyst.agent.md` | Consolidacao de anotacoes diversas |
| `the-planner` | `agents/communication/the-planner.agent.md` | Planejamento estrategico e roadmap |

### Dev (`agents/dev`)

| Agente | Arquivo | Foco Principal |
|---|---|---|
| `dev-loop-executor` | `agents/dev/dev-loop-executor.agent.md` | Execucao do Dev Loop (PROMPT_*.md) |
| `prompt-crafter` | `agents/dev/prompt-crafter.agent.md` | Descoberta guiada + geracao de PROMPT |

### Domain (`agents/domain`)

| Agente | Arquivo | Foco Principal |
|---|---|---|
| `aide-slide-builder` | `agents/domain/aide-slide-builder.agent.md` | Criacao de slides HTML no design system AIDE |
| `aide-slide-fixer` | `agents/domain/aide-slide-fixer.agent.md` | Correcao cirurgica de slides (minimo diff) |
| `aide-slide-planner` | `agents/domain/aide-slide-planner.agent.md` | Planejamento de mapa de slides |
| `aide-slide-reviewer` | `agents/domain/aide-slide-reviewer.agent.md` | Revisao de qualidade de slides (read-only) |
| `claude-to-copilot-commands` | `agents/domain/claude-to-copilot-commands.agent.md` | Migracao de comandos `.claude/commands` -> `.github/prompts` |
| `claude-to-copilot-migrator` | `agents/domain/claude-to-copilot-migrator.agent.md` | Migracao de agentes `.claude/agents` -> `.github/agents` |
| `crewai-specialist` | `agents/domain/crewai-specialist.agent.md` | Orquestracao multiagente com CrewAI |
| `shopagent-builder` | `agents/domain/shopagent-builder.agent.md` | Construcao de componentes do dominio ShopAgent |
| `sql-query-reviewer` | `agents/domain/sql-query-reviewer.agent.md` | Revisao de SQL para Postgres/Supabase |

### Exploration (`agents/exploration`)

| Agente | Arquivo | Foco Principal |
|---|---|---|
| `codebase-explorer` | `agents/exploration/codebase-explorer.agent.md` | Exploracao de arquitetura e saude do repositorio |
| `kb-architect` | `agents/exploration/kb-architect.agent.md` | Criacao/auditoria de dominios de Knowledge Base |

### Workflow SDD (`agents/workflow`)

| Agente | Arquivo | Fase |
|---|---|---|
| `brainstorm-agent` | `agents/workflow/brainstorm.agent.md` | Phase 0 - Brainstorm |
| `define-agent` | `agents/workflow/define.agent.md` | Phase 1 - Define |
| `design-agent` | `agents/workflow/design.agent.md` | Phase 2 - Design |
| `build-agent` | `agents/workflow/build.agent.md` | Phase 3 - Build |
| `ship-agent` | `agents/workflow/ship.agent.md` | Phase 4 - Ship |
| `iterate-agent` | `agents/workflow/iterate.agent.md` | Cross-phase - Iterate |

### Template (`agents/_template.agent.md`)

Template base para criar novos agentes com frontmatter, exemplos de uso e convencoes de ferramentas.

---

## Exemplo de Uso - Dev Agentic Loop

### 1) Exploracao inicial de notas

```text
Inspecione pasta @notes/ e:
1) Gere um pequeno sumario
2) Key decision points
3) Arquitetura tecnica do projeto
4) Pain points
```

### 2) Gerar especificacoes a partir de reunioes

```text
Utilize o agente @agents/communication/meeting-analyst.agent.md para analisar todas as anotacoes da pasta @notes/
e crie o documento detalhado @notes/summary-requirements.md
```

### 3) Gerar requerimentos tecnicos de design

```text
Utilize o agente @agents/communication/meeting-analyst.agent.md para analisar @notes/summary-requirements.md.

Extraia os requisitos do prototipo local em Python para extracao de invoices:
1. Separe requisitos funcionais FR.* relevantes para extracao
2. Identifique schemas Pydantic citados
3. Liste stack tecnologica decidida (LLM, validation, output format)
4. Defina criterios de aceite mensuraveis

Output: documento tecnico limpo (ate 100 linhas)
Write to: design/invoice-extractor-requirements.md
```

### 4) Gerar design tecnico do projeto

```text
Crie um design tecnico em 5 steps para o pipeline de extracao:
1. ARQUITETURA: diagrama ASCII de dataflow
2. COMPONENTES: modulos (processar imagem, gateway LLM, validator)
3. SCHEMAS: modelos Pydantic com field types
4. VALIDACAO: estrategia de 3 camadas
5. ESTRUTURA DOS ARQUIVOS: layout antes/depois processamento
6. CHECKLIST DE IMPLEMENTACAO: fases setup, core, tests

Keep it actionable - this feeds directly into the dev loop.
Write to: design/invoice-extractor-design.md
```

### 5) Executar desenvolvimento

```bash
@dev Build o invoice-extractor a partir do design/invoice-extractor-design.md
Target: src/invoice-extractor

Paths de dados
- data/input/  -> ler arquivos HTML
- data/output/ -> escrever parquet resultante
- data/store/  -> arquivar processados

Inclua CLI com comando de extrair e testes com pytest em tests/

@dev .github/dev/tasks/PROMPT_INVOICE_EXTRACTOR.md --dry-run
@dev .github/dev/tasks/PROMPT_INVOICE_EXTRACTOR.md
```

---

## Exemplo de Uso - SDD (Spec-Driven Development)

Fluxo completo com os prompts de workflow:

```bash
# 0) brainstorm completo
/workflow_brainstorm arquivo_requirements.md

# 1) define requisitos
/workflow_define {BRAINSTORM_ARQUIVO_GERADO.md}

# 2) define arquitetura
/workflow_design {DEFINE_ARQUIVO_GERADO.md}

# 3) constroi a aplicacao
/workflow_build {DESIGN_ARQUIVO_GERADO.md}

# 4) publica/arquiva feature
/workflow_ship
```

Iteracao de contexto em qualquer momento:

```bash
/workflow_iterate "Corrija tal coisa..."
```

---

## Prompts Principais

| Prompt | Arquivo | Objetivo |
|---|---|---|
| `/dev` | `prompts/dev.prompt.md` | Entrypoint do Dev Loop (craft + execute) |
| `/workflow_brainstorm` | `prompts/workflow_brainstorm.prompt.md` | Fase 0 |
| `/workflow_define` | `prompts/workflow_define.prompt.md` | Fase 1 |
| `/workflow_design` | `prompts/workflow_design.prompt.md` | Fase 2 |
| `/workflow_build` | `prompts/workflow_build.prompt.md` | Fase 3 |
| `/workflow_ship` | `prompts/workflow_ship.prompt.md` | Fase 4 |
| `/workflow_iterate` | `prompts/workflow_iterate.prompt.md` | Atualizacao cross-phase |

---

## Referencias Rapidas

- Dev Loop: `dev/README.md`
- SDD: `sdd/README.md`
- Instrucoes gerais: `copilot-instructions.md`
