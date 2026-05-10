# Plan de Optimizacion del Sistema de Trabajo

**Fecha:** 2026-03-23
**Ultima actualizacion:** 2026-03-23 (post-consolidacion)
**Estado:** EN PROGRESO
**Autor:** Brain Pipeline (system-optimization)

---

## Diagnostico del Sistema Actual (Post-Consolidacion)

### Cambios Detectados desde la Propuesta Inicial

Se realizo una consolidacion importante en `claude-code-template/.claude/`:

#### Skills Eliminadas (73 -> 66, -7 skills)
| Skill eliminada | Reemplazada por |
|-----------------|-----------------|
| engram-memory | **memory** (skill unificada dual: Engram + YAML) |
| session-memory | **memory** (consolidada en skill unica) |
| hipoteca-espana | **vivienda-espana** (skill unificada hipoteca + VPO/VPC) |
| proteccion-oficial-espana | **vivienda-espana** (consolidada) |
| obsidian-sync | **obsidian** (skill unificada) |
| obsidian-vault | **obsidian** (consolidada) |
| playwright-cli | **playwright** (skill unificada: CLI + Guide + MCP) |
| playwright-cli-guide | **playwright** (consolidada) |
| playwright-mcp | **playwright** (consolidada) |
| informe-reunion | **meeting-intelligence** (reemplazada) |
| meeting-processor | **meeting-intelligence** (consolidada) |
| youtube-search | **yt-search** (consolidada, eliminada duplicada) |

**Patron:** Consolidacion de skills duplicadas/fragmentadas en skills unificadas mas potentes.

#### Commands Eliminados (42 -> 35, -7 commands)
| Command eliminado | Razon |
|-------------------|-------|
| buscar-piso | Consolidado en **buscar-vivienda** |
| buscar-vpc | Consolidado en **buscar-vivienda** |
| content-pipeline | Eliminado (reemplazado por content-factory-v2 skill) |
| plan_agent_team | Consolidado en **plan-task** o **prompt-plan** |
| plan_w_team | Consolidado en **plan-task** o **prompt-plan** |
| prompt-plan-agents | Consolidado en **prompt-plan** |

#### Agents Modificados (11 de 16)
Todos los agents especializados fueron actualizados:
backend, frontend, gentleman, infra, testing, security-reviewer, quality-reviewer, codebase-analyst, claude-skills-architect, aepd-consultant, marketing-expert

#### Commands Modificados (9 de 35)
audit-system, brain, browser-test, build_team, implement-ecosystem, optimize-layer, plan-task, prompt-plan

### Inventario Actualizado

| Componente | Antes | Ahora | Delta |
|------------|-------|-------|-------|
| Skills | 73 | 66 | -7 (consolidacion) |
| Agents | 16 | 16 | 0 (11 modificados) |
| Commands | 42 | 35 | -7 (consolidacion) |
| Hooks | 14 | 14 | 0 |
| Rules | 3 | 3 | 0 |
| Skills globales | 15 | 15 | 0 (pendiente eliminar overlap) |

### Distribucion Actual (Symlinks)
| Repos con symlinks | 11 | Cargan TODO: 66 skills + 35 commands |
| Repos independientes | 4 | Cargan solo lo necesario |

### Problemas Criticos

| # | Problema | Severidad | Estado |
|---|---------|-----------|--------|
| 1 | **API key expuesta** en settings.local.json | CRITICO | **RESUELTO** - Key removida, .pi/ destrackeado, .gitignore actualizado |
| 2 | **Contaminacion de contexto**: todos los repos cargan 66 skills | ALTO | PENDIENTE - Mejorado de 88 a 66 pero aun excesivo |
| 3 | **Sin aislamiento**: 1 error en template rompe 11 repos | ALTO | PENDIENTE |
| 4 | **Duplicacion global/template**: 15 skills en ambos | MEDIO | PENDIENTE |
| 5 | **library.yaml vacio**: the-library existe pero no se usa | MEDIO | PENDIENTE |

---

## Principios de Diseno (IndyDevDan)

### Los 3 Primitivos Agenticos (Jerarquia)
```
PROMPTS/COMMANDS (Orquestacion: cuando y como)
       |
   AGENTS (Escala: paralelismo y delegacion)
       |
   SKILLS (Capacidades puras: hacer una cosa bien)
```

### Filosofia Core
1. **Private-first**: Las capacidades especializadas son IP valiosa
2. **Trust = Transparencia**: Saber linea por linea que ejecuta cada agente
3. **Catalogo, no monolito**: Referencias lazy-loaded, no "todo siempre"
4. **Cookbook pattern**: Documentacion que los agentes pueden leer y ejecutar
5. **Consolidar > Fragmentar**: Skills unificadas > multiples skills fragmentadas (validado por la consolidacion reciente)
6. **Progresion**: Agente base -> Mejor -> Mas -> Custom -> **Orquestador**

### Insight Clave
> Symlinks y The Library NO son mutuamente excluyentes.
> Symlinks = velocidad de desarrollo local.
> Library = distribucion + catalogacion + descubrimiento.
> El sistema optimo usa AMBOS.

---

## Plan de Optimizacion en 4 Fases

### FASE 0: Seguridad Inmediata - COMPLETADA

**Acciones realizadas:**
- [x] API key removida de `settings.local.json`
- [x] `.pi/` destrackeado de git (65 archivos)
- [x] `.claude/settings.local.json` agregado a `.gitignore`
- [x] `.pi/` agregado a `.gitignore`
- [x] Verificado: 0 API keys reales en `.claude/`

**Pendiente del usuario:**
- [ ] Rotar API key en console.anthropic.com (key en historial git)
- [ ] Commitear cambios staged en claude-code-template

---

### FASE 1: Categorizacion de Skills (1-2 dias)

**Objetivo**: Clasificar las 66 skills en dominios para permitir seleccion granular.

**Taxonomia Actualizada (post-consolidacion):**

| Categoria | Skills (66 total) | Repos que las necesitan |
|-----------|--------|----------------------|
| **core** (8) | prime, continue-session, memory, auto-routing, phase-metrics, hooks, agent-snapshot, take-the-wheel | TODOS |
| **development** (12) | fastapi, clean-arch, react-19, nextjs, tailwind, pydantic-ai, supabase, rls, stripe-integration, pytest, github, mcp-tools | ideas, vitaeon, ekklosion |
| **content** (7) | linkedin-publisher, typefully-drafts, content-factory-v2, marketing-content, sales-outreach, edit-carousel, remotion-videos | linkedin-content, studiotek-landing |
| **documentation** (4) | markdown, obsidian, meeting-intelligence, pdf-to-docx | ai_docs, the-library |
| **infrastructure** (5) | deploy, monitor, cloudflare-tunnel, tmux, organize-repo | vitaeon, ideas, studiotek-landing |
| **diagrams** (5) | svg-premium, mermaid, excalidraw-diagram-skill, system-blueprint, automation-architect | TODOS (utility) |
| **security** (3) | owasp, aepd-privacidad, code-analysis | auditorias, compliance |
| **ai-tooling** (8) | agent-builder, claude-agents-sdk, claude-code-commands, subagent-swarm, roles-focus, autoresearch, notebooklm, yt-search | the-library, ai_docs |
| **clients** (4) | vivienda-espana, spanish-invoicing, commercial-proposal, studiotek-landing-enhancer | Proyecto especifico |
| **design** (3) | pencil-design-improver, landing-image-generator, ideas-design | studiotek-landing, vitaeon-landing, ideas |
| **pipeline** (4) | prd, sdd, skill-creator, notion | Cuando se usa Golden Path |
| **browser** (1) | playwright | Testing E2E |

**Validacion:** 8+12+7+4+5+5+3+8+4+3+4+1 = 64 (2 skills sin categorizar: seo-toolkit → content, pr-review → development) = **66 total**

**Perfiles por proyecto (actualizados):**

```yaml
profiles:
  linkedin-content: [core, content]                    # ~15 skills
  ideas: [core, development, infrastructure, pipeline, design]  # ~32 skills
  vitaeon: [core, development, infrastructure]          # ~25 skills
  the-library: [core, documentation, ai-tooling]       # ~20 skills
  studiotek-landing: [core, content, design, infrastructure]    # ~23 skills
  mr_champagne: [core, development]                    # ~20 skills
  ekklosion: [core, development, infrastructure]       # ~25 skills
  vesta: [core, development]                           # ~20 skills
  Transportes-Bejarano: [core, clients]                # ~12 skills
  ai_docs: [core, documentation, ai-tooling, diagrams] # ~25 skills
```

**Gate:** Archivo `skill-categories.yaml` creado con las 66 skills categorizadas.

---

### FASE 2: Arquitectura Hibrida Symlinks + Library (3-5 dias)

#### Paso 2.1: Eliminar duplicacion global/template

Las 15 skills en `~/.claude/skills/` ya estan en el template. Accion:
- Eliminar de `~/.claude/skills/` las que ya estan en template
- Mantener en `~/.claude/` solo: CLAUDE.md, settings.json, agents/svg-diagram-expert.md, commands/generar-contrato.md

#### Paso 2.2: Poblar library.yaml

Registrar las **66 skills + 16 agents + 35 commands** en `the-library/library.yaml`:

```yaml
library:
  skills:
    - name: prime
      description: Fast project context loader
      source: /Users/cristianbejaranomendez/Documents/GitHub/claude-code-template/.claude/skills/prime/SKILL.md

    - name: memory
      description: Memoria persistente dual (Engram MCP + YAML domain-experts)
      source: /Users/cristianbejaranomendez/Documents/GitHub/claude-code-template/.claude/skills/memory/SKILL.md

    - name: playwright
      description: Browser automation (CLI + Guide + MCP, 3 modes)
      source: /Users/cristianbejaranomendez/Documents/GitHub/claude-code-template/.claude/skills/playwright/SKILL.md

    - name: vivienda-espana
      description: Asesor integral de vivienda para Espana (hipoteca + VPO/VPC)
      source: /Users/cristianbejaranomendez/Documents/GitHub/claude-code-template/.claude/skills/vivienda-espana/SKILL.md

    - name: obsidian
      description: Obsidian vault management (setup, daily notes, Dataview, Zettelkasten)
      source: /Users/cristianbejaranomendez/Documents/GitHub/claude-code-template/.claude/skills/obsidian/SKILL.md

    - name: meeting-intelligence
      description: Meeting processing and intelligence (transcriptions, entities, reports)
      source: /Users/cristianbejaranomendez/Documents/GitHub/claude-code-template/.claude/skills/meeting-intelligence/SKILL.md

    # ... 60 mas

  agents:
    - name: backend
      description: Senior Backend Engineer (FastAPI, SQLAlchemy, PostgreSQL, Clean Architecture)
      source: /Users/cristianbejaranomendez/Documents/GitHub/claude-code-template/.claude/agents/backend.md

    # ... 15 mas

  commands:
    - name: brain
      description: Cerebro agentico - clasifica y ejecuta pipeline AIOS de 8 fases
      source: /Users/cristianbejaranomendez/Documents/GitHub/claude-code-template/.claude/commands/brain.md

    - name: buscar-vivienda
      description: Busqueda de vivienda en Barcelona (mercado libre + protegida VPC)
      source: /Users/cristianbejaranomendez/Documents/GitHub/claude-code-template/.claude/commands/buscar-vivienda.md

    # ... 33 mas
```

#### Paso 2.3: Modelo de Symlinks Selectivos

**Alternativa recomendada** (minimo cambio):
Usar `.claude/skill-profile.yaml` en cada repo para filtrar:

```yaml
# linkedin-content/.claude/skill-profile.yaml
active_categories: [core, content]
```

**Gate:** library.yaml poblado con al menos 20 entries. skill-categories.yaml creado.

---

### FASE 3: Workflow Optimizado (ongoing)

#### 3.1 Flujo de Desarrollo de Nueva Skill
```
1. Crear skill en claude-code-template/.claude/skills/nueva-skill/
2. Asignar categoria en skill-categories.yaml
3. Registrar en library.yaml: /library add nueva-skill
4. Los repos con symlink la ven automaticamente
5. Otros dispositivos: /library use nueva-skill
```

#### 3.2 Flujo de Consolidacion (validado por los cambios recientes)
```
1. Identificar skills fragmentadas (ej: obsidian-sync + obsidian-vault)
2. Crear skill unificada (ej: obsidian) con todos los modos
3. Eliminar skills viejas
4. Actualizar library.yaml: /library remove old + /library add new
5. Actualizar skill-categories.yaml
```

#### 3.3 Flujo para Nuevo Proyecto
```
1. Crear repo
2. Crear .claude/skill-profile.yaml con categorias necesarias
3. Symlink a template (como ahora)
4. Si necesita algo puntual: /library use skill-especifica
```

#### 3.4 Eliminar Overlap Global
```bash
# Eliminar las 15 skills duplicadas de ~/.claude/skills/
# que ya estan en template (auto-routing, autoresearch, etc.)
# Mantener solo: CLAUDE.md, settings.json, svg-diagram-expert.md, generar-contrato.md
```

---

## Resumen de Impacto

| Metrica | Antes (original) | Post-consolidacion | Objetivo final |
|---------|------|------|-------|
| Skills en template | 73 | 66 | 66 (ya optimizado) |
| Commands en template | 42 | 35 | 35 (ya optimizado) |
| Skills cargadas (linkedin-content) | 88 | 81 | ~15 (core + content) |
| Skills cargadas (ideas) | 88 | 81 | ~32 (perfil selectivo) |
| Tokens de contexto por skills | ~50K | ~45K | ~15-20K |
| Secretos expuestos | 1 API key x11 | 0 | 0 |
| Duplicacion global/template | 15 x2 | 15 x2 | 0 |
| library.yaml entries | 0 | 0 | 117 (66+16+35) |

---

## Diagrama de Arquitectura Propuesta

```
                    GitHub (the-library repo)
                    library.yaml (catalogo: 66 skills + 16 agents + 35 commands)
                           |
                    +-----------+
                    |           |
              LOCAL MACHINE    REMOTE DEVICE
                    |           |
         claude-code-template   /library sync
         /.claude/              |
         ├─ skills/ (66)        ~/.claude/skills/
         │  ├─ memory           (solo lo instalado)
         │  ├─ playwright
         │  ├─ obsidian
         │  └─ ... (consolidadas)
         ├─ agents/ (16)
         ├─ commands/ (35)
         └─ hooks/
              |
    +---------+---------+---------+
    |         |         |         |
linkedin  ideas    vitaeon    ekklosion
 profile:  profile:  profile:  profile:
 core+     core+     core+     core+
 content   dev+      dev+      dev+
 (~15)     infra+    infra     infra
           pipeline  (~25)     (~25)
           (~32)
```

---

## Orden de Ejecucion

1. **FASE 0** - Seguridad - **COMPLETADA**
2. **FASE 1** - Categorizacion (crear skill-categories.yaml) - PENDIENTE
3. **FASE 2.2** - Poblar library.yaml (66+16+35 entries) - PENDIENTE
4. **FASE 2.1** - Eliminar 15 skills duplicadas de global - PENDIENTE
5. **FASE 2.3** - skill-profile.yaml por proyecto - PENDIENTE
6. **FASE 3** - Workflows documentation - ONGOING

---

## Aprendizajes de la Consolidacion

La consolidacion realizada valida el principio de IndyDevDan:
> "Separar capacidades puras (skills) de orquestacion, pero consolidar skills que hacen lo mismo en diferentes modos."

**Patron exitoso:** Unificar N skills fragmentadas en 1 skill con multiples modos:
- `playwright` (3 modes: CLI, Guide, MCP) reemplaza 3 skills separadas
- `memory` (dual: Engram + YAML) reemplaza 2 skills separadas
- `vivienda-espana` (hipoteca + VPO) reemplaza 2 skills separadas
- `obsidian` (sync + vault) reemplaza 2 skills separadas

**Resultado neto:** -7 skills, -7 commands = menos contexto, misma funcionalidad.

---

## Auditoria de Compatibilidad (Post-Consolidacion)

Auditoria completa de referencias rotas tras la consolidacion de skills/commands.

### Resultado: La optimizacion es SEGURA

Los directorios criticos de runtime estan limpios:

| Directorio | Estado | Detalle |
|------------|--------|---------|
| `scripts/` | LIMPIO | 0 referencias a items eliminados |
| `.claude/hooks/` | LIMPIO | 0 referencias rotas |
| `.claude/rules/` | LIMPIO | 0 referencias rotas |
| `.claude/agents/` | LIMPIO | Los 11 modificados ya estan actualizados |
| `.claude/commands/` | LIMPIO | Skills consolidadas documentan origen correctamente |
| `.claude/skills/` | LIMPIO | Nuevas skills merged usan `*Merged from X + Y*` |

### Fixes Requeridos

#### CRITICO: Justfile (3 commands eliminados aun referenciados)

| Referencia rota | Comando actual | Fix |
|-----------------|---------------|-----|
| `/prompt-plan-agents {{prompt}}` | Consolidado en `/prompt-plan` | Cambiar a `/prompt-plan --mode=agents {{prompt}}` |
| `/plan_w_team {{prompt}}` | Consolidado en `/plan-task` | Cambiar a `/plan-task {{prompt}}` |
| `/plan_agent_team {{prompt}}` | Consolidado en `/plan-task` | Cambiar a `/plan-task --mode=agents {{prompt}}` |

**Impacto:** Si alguien ejecuta `just plan-team` o similar, fallara. Fix es trivial (3 lineas).

#### MENOR: brain.md routing table (paths skill vs command)

| Linea | Referencia | Issue | Fix |
|-------|-----------|-------|-----|
| 30 | `context-install/SKILL.md` | Es command, no skill | Referenciar como command |
| 35 | `plan-task/SKILL.md` | Es command, no skill | Referenciar como command |
| 36 | `build/SKILL.md` | Es command, no skill | Referenciar como command |
| 37 | `audit-system/SKILL.md` | Es command, no skill | Referenciar como command |

**Impacto:** Bajo. brain.md despacha via nombre de skill/command, no via path literal. Es un issue preexistente, no causado por la consolidacion.

#### INFORMATIVO: Documentacion historica en ai_docs/ (NO afecta runtime)

Planes historicos ya ejecutados que referencian nombres viejos:

| Archivo | Referencia vieja | Estado |
|---------|-----------------|--------|
| `ai_docs/plans/install-engram-mcp-9-projects.md` | engram-memory | Plan ejecutado, solo historico |
| `ai_docs/plans/install-playwright-cli-and-skill.md` | playwright-cli-guide | Plan ejecutado, obsoleto |
| `ai_docs/plans/proteccion-oficial-espana-skill.md` | proteccion-oficial-espana | Plan ejecutado, obsoleto |
| `ai_docs/plans/buscar-vpc-command.md` | buscar-vpc | Plan ejecutado, obsoleto |
| `ai_docs/plans/cct1-4-layer-automation-system.md` | session-memory, playwright-cli-guide | Inventario historico |
| `docs/4-LAYER-ARCHITECTURE.md` | playwright-cli-guide, plan_w_team | Documentacion desactualizada |
| `docs/CCT1-GUIDE.md` | plan_w_team, plan_agent_team | Documentacion desactualizada |
| `ai_docs/ideacion/work-systems-e2e-ideacion.md` | Varios commands viejos | Inventario historico |
| `specs/research/hooks-inventory.md` | plan_w_team, plan_agent_team | Research historico |
| `content-factory-v2/SKILL.md` | /content-pipeline | Referencia en documentacion |
| `automation-architect/referencias/patrones-automatizacion.md` | /content-pipeline | Referencia en patrones |

**Impacto:** Ninguno en runtime. Son documentos de planificacion/investigacion ya ejecutados. Pueden limpiarse como tarea de housekeeping futura pero no bloquean nada.

### Conclusion de la Auditoria

**Bloqueante para continuar:** Solo el Justfile (3 lineas).
**Todo lo demas:** O esta limpio o es documentacion historica sin impacto en runtime.
