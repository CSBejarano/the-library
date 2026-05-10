# Plan: Video Learning Pipeline — IndyDevDan YouTube Video

## Task Description
Crear un pipeline de aprendizaje completo a partir de un video de IndyDevDan (disler) sobre "The Library". El pipeline extrae metadata del video, lo sube a Google NotebookLM, genera 5 tipos de materiales de estudio, realiza preguntas contextualizadas a los 19 proyectos de Cristian, y descarga todos los artefactos localmente.

- **Video**: https://www.youtube.com/watch?v=_vpNQ6IwP9w
- **Repo del video**: https://github.com/disler/the-library (el repo donde estamos es el clon)
- **IndyDevDan (disler)**: Referente principal de Cristian en IA y agentics

## Objective
Al completar este plan, Cristian tendrá:
1. Metadata del video documentada
2. Un notebook en NotebookLM con el video indexado como fuente
3. 5 artefactos de aprendizaje generados (briefing doc, study guide, mind map, quiz, flashcards)
4. 3 respuestas contextualizadas sobre cómo aplicar las enseñanzas a sus proyectos
5. Todos los artefactos descargados en `logs/notebooklm/`

## Problem Statement
Cristian quiere extraer el máximo valor de aprendizaje de videos de su referente IndyDevDan y conectar esas enseñanzas directamente con sus ~19 proyectos activos en GitHub. El proceso manual de ver un video, tomar notas, y aplicarlas es lento y pierde contexto. Se necesita un pipeline automatizado que genere materiales de estudio estructurados y contextualizados.

## Solution Approach
Pipeline secuencial de 7 pasos usando dos skills existentes (`/youtube-search` para metadata, `/notebooklm` para todo lo demás). Los artefactos se generan en orden de fiabilidad (primero los que nunca fallan: mind-map, reports; después los propensos a rate-limiting: quiz, flashcards). El chat contextualizado se ejecuta en paralelo con la generación de artefactos. Todo se descarga a una carpeta local para referencia offline.

## GitHub Workflow

**NO APLICA** — Este plan es un pipeline de aprendizaje/investigación. No se modifica código en ningún repositorio. No se requiere issue, branch, ni PR. Los outputs son artefactos de aprendizaje descargados a `logs/notebooklm/`.

## Relevant Files

- `~/.claude/skills/yt-search/scripts/search.py` — Script de búsqueda YouTube (yt-dlp). Extrae metadata: título, canal, duración, views, fecha
- `~/.claude/skills/notebooklm/SKILL.md` — Definición completa del skill NotebookLM con todos los comandos CLI
- `library.yaml` — Catálogo del Library (contexto del proyecto para las preguntas)
- `SKILL.md` — Definición del meta-skill Library (contexto para las preguntas)
- `README.md` — Documentación del proyecto (contexto para las preguntas)

### New Files
- `logs/notebooklm/` — Directorio destino para todos los artefactos descargados
- `logs/notebooklm/metadata.json` — Metadata del video
- `logs/notebooklm/briefing-doc.md` — Briefing document
- `logs/notebooklm/study-guide.md` — Study guide
- `logs/notebooklm/mind-map.json` — Mind map JSON
- `logs/notebooklm/quiz.md` — Quiz en markdown
- `logs/notebooklm/flashcards.md` — Flashcards en markdown
- `logs/notebooklm/chat-responses.md` — Respuestas del chat contextualizado

## Implementation Phases

### Phase 1: Foundation (Metadata + Notebook Setup)
Obtener metadata del video y crear el notebook en NotebookLM con el video como fuente indexada.

### Phase 2: Core Generation (Artifacts + Chat)
Generar los 5 artefactos de aprendizaje y ejecutar las 3 preguntas contextualizadas. Mind-map primero (instant), luego reports (reliable), luego quiz/flashcards (rate-limit prone). Chat en paralelo.

### Phase 3: Download & Validate
Descargar todos los artefactos a `logs/notebooklm/` y validar completitud.

## Team Orchestration

- Operas como el líder del equipo y orquestas al equipo para ejecutar el plan.
- NUNCA operas directamente. Usas las herramientas `Task` y `Task*` para desplegar a los miembros del equipo.
- Este pipeline es mayormente secuencial con oportunidades puntuales de paralelismo.

### Team Members

- Builder (researcher-metadata)
  - Name: researcher-metadata
  - Role: Obtener metadata del video YouTube usando el script search.py de yt-search
  - Agent Type: builder
  - Skills: /youtube-search
  - Resume: false

- Builder (notebooklm-setup)
  - Name: notebooklm-setup
  - Role: Crear notebook en NotebookLM, agregar video como fuente, esperar indexación
  - Agent Type: builder
  - Skills: /notebooklm
  - Resume: true

- Builder (artifact-generator)
  - Name: artifact-generator
  - Role: Generar los 5 artefactos de aprendizaje en NotebookLM (briefing, study-guide, mind-map, quiz, flashcards)
  - Agent Type: builder
  - Skills: /notebooklm
  - Resume: true

- Builder (contextual-chat)
  - Name: contextual-chat
  - Role: Realizar 3 preguntas contextualizadas sobre los proyectos de Cristian al notebook
  - Agent Type: builder
  - Skills: /notebooklm
  - Resume: true

- Builder (artifact-downloader)
  - Name: artifact-downloader
  - Role: Descargar todos los artefactos generados a la carpeta local
  - Agent Type: builder
  - Skills: /notebooklm
  - Resume: true

- Validator (final-validator)
  - Name: final-validator
  - Role: Verificar que todos los artefactos fueron generados y descargados correctamente
  - Agent Type: validator
  - Skills: N/A
  - Resume: false

## Step by Step Tasks

### 1. Obtener Metadata del Video
- **Task ID**: get-video-metadata
- **Depends On**: none
- **Assigned To**: researcher-metadata
- **Agent Type**: builder
- **Skills**: /youtube-search
- **Parallel**: false
- **Purpose**: Extraer metadata del video de YouTube para tener título exacto, canal, duración y contexto
- **Variables**:
  - VIDEO_URL: `https://www.youtube.com/watch?v=_vpNQ6IwP9w`
  - SEARCH_SCRIPT: `~/.claude/skills/yt-search/scripts/search.py`
  - OUTPUT_DIR: `logs/notebooklm/`
- **Code Structure**: Script Python `search.py` que usa yt-dlp para buscar videos. Acepta query string y flags `--count N`, `--months N`, `--no-date-filter`
- **Instructions**:
  1. Crear directorio de output: `mkdir -p logs/notebooklm/`
  2. Ejecutar búsqueda: `python ~/.claude/skills/yt-search/scripts/search.py "IndyDevDan the library skill distribution" --count 5 --no-date-filter`
  3. Identificar el video correcto por URL o título que coincida con `_vpNQ6IwP9w`
  4. Si no aparece en búsqueda, intentar con: `python ~/.claude/skills/yt-search/scripts/search.py "disler the library meta-skill" --count 10 --no-date-filter`
  5. Guardar la metadata completa del video en `logs/notebooklm/metadata.json`
  6. Extraer y reportar: título, canal, duración, views, fecha de publicación
- **Workflow**: mkdir → search.py con query → identificar video → guardar metadata JSON → reportar
- **Examples**: `python search.py "IndyDevDan agentics" --count 5 --no-date-filter` → JSON con título, canal, views, duración
- **Report**: Archivo `logs/notebooklm/metadata.json` creado con metadata del video. Reportar título exacto del video y duración.

### 2. Crear Notebook y Agregar Video como Fuente
- **Task ID**: setup-notebook
- **Depends On**: get-video-metadata
- **Assigned To**: notebooklm-setup
- **Agent Type**: builder
- **Skills**: /notebooklm
- **Parallel**: false
- **Purpose**: Crear un notebook en NotebookLM con nombre descriptivo y agregar el video de YouTube como fuente indexada
- **Variables**:
  - VIDEO_URL: `https://www.youtube.com/watch?v=_vpNQ6IwP9w`
  - NOTEBOOK_TITLE: Se construye dinámicamente con el título del video de la tarea anterior (ej. "IndyDevDan: The Library — Private Skill Distribution")
  - NOTEBOOK_ID: Se captura del output de `notebooklm create --json`
  - SOURCE_ID: Se captura del output de `notebooklm source add --json`
- **Code Structure**: CLI `notebooklm` con subcomandos: create, use, source add, source wait, source list
- **Instructions**:
  1. Verificar autenticación: `notebooklm status`
  2. Si auth falla, ejecutar: `notebooklm auth check` y reportar al líder
  3. Crear notebook: `notebooklm create "IndyDevDan: [título del video]" --json` → capturar notebook_id
  4. Set context: `notebooklm use <notebook_id>`
  5. Agregar video como fuente: `notebooklm source add "https://www.youtube.com/watch?v=_vpNQ6IwP9w" --json` → capturar source_id
  6. Esperar indexación: `notebooklm source wait <source_id> --timeout 600`
  7. Verificar fuente lista: `notebooklm source list --json` → confirmar status "ready"
  8. Si indexación falla o timeout: reportar error al líder, NO continuar
- **Workflow**: status → create notebook → use → source add → source wait → verify ready
- **Examples**:
  ```bash
  notebooklm create "IndyDevDan: The Library" --json
  # → {"id": "abc123...", "title": "IndyDevDan: The Library"}
  notebooklm source add "https://www.youtube.com/watch?v=_vpNQ6IwP9w" --json
  # → {"source_id": "def456...", "status": "processing"}
  ```
- **Report**: Notebook ID, Source ID, status de indexación (ready/error). Estos IDs son CRÍTICOS para las siguientes tareas.

### 3. Generar Artefactos de Aprendizaje (Reliable)
- **Task ID**: generate-reliable-artifacts
- **Depends On**: setup-notebook
- **Assigned To**: artifact-generator
- **Agent Type**: builder
- **Skills**: /notebooklm
- **Parallel**: true (puede correr en paralelo con contextual-chat)
- **Purpose**: Generar los 3 artefactos fiables: mind-map (instant), briefing-doc (report), study-guide (report)
- **Variables**:
  - NOTEBOOK_ID: Del output de setup-notebook
  - LANGUAGE: es (español para los reports)
- **Code Structure**: CLI `notebooklm` con subcomandos: generate mind-map, generate report, artifact list, artifact wait
- **Instructions**:
  1. Verificar context activo: `notebooklm status` (debe mostrar el notebook correcto)
  2. Si no hay context: `notebooklm use <NOTEBOOK_ID>`
  3. **Mind Map** (instant/sync):
     - `notebooklm generate mind-map --json`
     - Capturar artifact_id del mind-map
     - Mind-map es instantáneo, no necesita wait
  4. **Briefing Doc**:
     - `notebooklm generate report --format briefing-doc --language es --json`
     - Capturar artifact_id
     - `notebooklm artifact wait <artifact_id> --timeout 900`
  5. **Study Guide**:
     - `notebooklm generate report --format study-guide --language es --json`
     - Capturar artifact_id
     - `notebooklm artifact wait <artifact_id> --timeout 900`
  6. Verificar todos completados: `notebooklm artifact list --json` → todos status "completed"
  7. Si alguno falla: retry una vez después de 60 segundos. Si sigue fallando, reportar error.
- **Workflow**: status → generate mind-map → generate briefing-doc → wait → generate study-guide → wait → verify all completed
- **Examples**:
  ```bash
  notebooklm generate mind-map --json
  # → instant result
  notebooklm generate report --format briefing-doc --language es --json
  # → {"task_id": "xyz...", "status": "pending"}
  notebooklm artifact wait xyz --timeout 900
  ```
- **Report**: Lista de artifact_ids generados con sus tipos y status. Todos deben ser "completed".

### 4. Generar Artefactos de Aprendizaje (Rate-Limit Prone)
- **Task ID**: generate-quiz-artifacts
- **Depends On**: generate-reliable-artifacts
- **Assigned To**: artifact-generator
- **Agent Type**: builder
- **Skills**: /notebooklm
- **Parallel**: false (secuencial para evitar rate limits)
- **Purpose**: Generar quiz y flashcards (propensos a rate-limiting, se ejecutan después de los fiables)
- **Variables**:
  - NOTEBOOK_ID: Del output de setup-notebook
- **Code Structure**: CLI `notebooklm` con subcomandos: generate quiz, generate flashcards, artifact wait
- **Instructions**:
  1. Verificar context activo: `notebooklm status`
  2. **Quiz** (medium difficulty):
     - `notebooklm generate quiz --difficulty medium --json`
     - Capturar artifact_id
     - `notebooklm artifact wait <artifact_id> --timeout 900`
     - Si rate-limited: esperar 5 minutos y retry UNA vez
  3. **Flashcards** (medium difficulty):
     - `notebooklm generate flashcards --difficulty medium --json`
     - Capturar artifact_id
     - `notebooklm artifact wait <artifact_id> --timeout 900`
     - Si rate-limited: esperar 5 minutos y retry UNA vez
  4. Verificar: `notebooklm artifact list --json`
  5. Si ambos fallan por rate-limit: reportar y marcar como "pendiente manual" (el usuario puede generarlos desde la web UI)
- **Workflow**: status → generate quiz → wait → generate flashcards → wait → verify
- **Examples**:
  ```bash
  notebooklm generate quiz --difficulty medium --json
  # → {"task_id": "abc...", "status": "pending"}
  # Si falla: esperar 300s y retry
  ```
- **Report**: artifact_ids de quiz y flashcards con status. Si alguno falló por rate-limit, indicar cuál para generación manual.

### 5. Chat Contextualizado — Aplicación a Proyectos
- **Task ID**: contextual-chat
- **Depends On**: setup-notebook
- **Assigned To**: contextual-chat
- **Agent Type**: builder
- **Skills**: /notebooklm
- **Parallel**: true (puede correr en paralelo con generate-reliable-artifacts)
- **Purpose**: Hacer 3 preguntas específicas al notebook sobre cómo aplicar las enseñanzas del video a los proyectos de Cristian
- **Variables**:
  - NOTEBOOK_ID: Del output de setup-notebook
  - OUTPUT_FILE: `logs/notebooklm/chat-responses.md`
  - PROJECTS_CONTEXT: Lista de 19 proyectos con descripciones (ver abajo)
- **Code Structure**: CLI `notebooklm ask` con flag `--new` para nueva conversación y `--json` para output estructurado
- **Instructions**:
  1. Verificar context: `notebooklm status`
  2. **Pregunta 1** (nueva conversación):
     ```bash
     notebooklm ask "What are the main teachings from this video and how can I apply them to a multi-agent system for private skill distribution? I have a project called 'the-library' which is a clone of this exact repo, and I use it to distribute skills, agents, and prompts across my ~19 GitHub projects." --new --json
     ```
  3. **Pregunta 2** (follow-up en misma conversación):
     ```bash
     notebooklm ask "What practices from this video can I implement immediately in my SaaS projects that use FastAPI, PydanticAI, and Supabase? My main SaaS projects are: 'ideas' (booking SaaS with WhatsApp + AI), 'vitaeon' (health SaaS with PydanticAI), and 'vesta' (audit pipeline with Whisper + Claude)." --json
     ```
  4. **Pregunta 3** (follow-up):
     ```bash
     notebooklm ask "How do the concepts in this video relate to building automation pipelines with Claude Code? I have projects like 'claude-code-template' (orchestrator SDK template), 'claude-agents-sdk-studio' (Claude Agents SDK), 'engram' (persistent memory for agents), and 'ai_docs' (multi-agent documentation system)." --json
     ```
  5. Compilar las 3 respuestas en un archivo markdown con formato:
     ```markdown
     # Chat Contextualizado — IndyDevDan Video
     ## Pregunta 1: Enseñanzas principales + multi-agent skill distribution
     [respuesta]
     ## Pregunta 2: Prácticas para SaaS con FastAPI/PydanticAI
     [respuesta]
     ## Pregunta 3: Automation pipelines con Claude Code
     [respuesta]
     ```
  6. Guardar en `logs/notebooklm/chat-responses.md`
- **Workflow**: status → ask Q1 --new → ask Q2 → ask Q3 → compile markdown → save file
- **Examples**:
  ```bash
  notebooklm ask "What are the key concepts?" --new --json
  # → {"answer": "...", "references": [...]}
  ```
- **Report**: Archivo `logs/notebooklm/chat-responses.md` creado con las 3 respuestas. Resumen de 1 línea de cada respuesta.

### 6. Descargar Todos los Artefactos
- **Task ID**: download-artifacts
- **Depends On**: generate-reliable-artifacts, generate-quiz-artifacts, contextual-chat
- **Assigned To**: artifact-downloader
- **Agent Type**: builder
- **Skills**: /notebooklm
- **Parallel**: false
- **Purpose**: Descargar todos los artefactos generados a la carpeta local `logs/notebooklm/`
- **Variables**:
  - NOTEBOOK_ID: Del output de setup-notebook
  - OUTPUT_DIR: `logs/notebooklm/`
  - ARTIFACT_IDS: Del output de generate-reliable-artifacts y generate-quiz-artifacts
- **Code Structure**: CLI `notebooklm download` con tipos: report, mind-map, quiz, flashcards
- **Instructions**:
  1. Verificar context: `notebooklm status`
  2. Verificar artefactos disponibles: `notebooklm artifact list --json`
  3. Descargar cada artefacto:
     ```bash
     # Briefing Doc
     notebooklm download report logs/notebooklm/briefing-doc.md

     # Study Guide (segundo report — usar -a <artifact_id> para especificar)
     notebooklm download report logs/notebooklm/study-guide.md -a <study_guide_artifact_id>

     # Mind Map
     notebooklm download mind-map logs/notebooklm/mind-map.json

     # Quiz (markdown format)
     notebooklm download quiz --format markdown logs/notebooklm/quiz.md

     # Flashcards (markdown format)
     notebooklm download flashcards --format markdown logs/notebooklm/flashcards.md
     ```
  4. Si algún artefacto no se generó (rate-limit), skip con warning
  5. Verificar que los archivos existen y tienen contenido: `ls -la logs/notebooklm/`
- **Workflow**: status → artifact list → download each → verify files exist
- **Examples**:
  ```bash
  notebooklm download report logs/notebooklm/briefing-doc.md
  # → File saved to logs/notebooklm/briefing-doc.md
  ```
- **Report**: Lista de archivos descargados con tamaños. Indicar si alguno falta por rate-limit.

### 7. Validación Final
- **Task ID**: validate-all
- **Depends On**: download-artifacts
- **Assigned To**: final-validator
- **Agent Type**: validator
- **Skills**: N/A
- **Parallel**: false
- **Purpose**: Verificar que TODOS los criterios de aceptación se cumplen y que los artefactos están completos
- **Variables**:
  - OUTPUT_DIR: `logs/notebooklm/`
  - EXPECTED_FILES: metadata.json, briefing-doc.md, study-guide.md, mind-map.json, quiz.md, flashcards.md, chat-responses.md
- **Code Structure**: Archivos en `logs/notebooklm/`
- **Instructions**:
  1. Verificar que el directorio `logs/notebooklm/` existe
  2. Listar todos los archivos: `ls -la logs/notebooklm/`
  3. Para cada archivo esperado, verificar:
     - Existe
     - Tiene contenido (tamaño > 0)
     - Formato correcto (JSON válido para .json, Markdown válido para .md)
  4. Leer `metadata.json` y verificar que tiene campos: título, canal, duración
  5. Leer `chat-responses.md` y verificar que tiene 3 secciones de preguntas/respuestas
  6. Contar artefactos generados vs esperados (mínimo 3 de 5 artefactos NotebookLM)
  7. Reportar tabla PASS/FAIL por criterio
- **Workflow**: ls directory → check each file → validate content → report table
- **Examples**: N/A (tarea de verificación)
- **Report**: Tabla con cada criterio y su resultado PASS/FAIL. Veredicto final: APROBADO o RECHAZADO con motivos.

## Acceptance Criteria

1. **Metadata obtenida**: `logs/notebooklm/metadata.json` existe con título, canal, duración del video
2. **Notebook creado**: Notebook existe en NotebookLM con el video como source indexada (status: ready)
3. **Mind Map generado**: `logs/notebooklm/mind-map.json` existe con estructura jerárquica
4. **Briefing Doc generado**: `logs/notebooklm/briefing-doc.md` existe con contenido en español
5. **Study Guide generado**: `logs/notebooklm/study-guide.md` existe con contenido en español
6. **Quiz generado** (best-effort): `logs/notebooklm/quiz.md` existe (puede fallar por rate-limit)
7. **Flashcards generadas** (best-effort): `logs/notebooklm/flashcards.md` existe (puede fallar por rate-limit)
8. **Chat contextualizado**: `logs/notebooklm/chat-responses.md` existe con 3 respuestas sobre los proyectos
9. **Mínimo 5 archivos**: Al menos 5 de 7 archivos descargados exitosamente

## Testing Strategy
- **Testing Framework**: Validación por inspección de archivos (no hay código que testear)
- **Validation Commands**: `ls -la logs/notebooklm/`, `cat` de cada archivo para verificar contenido
- **Coverage Target**: 7/7 archivos idealmente, mínimo aceptable 5/7 (quiz y flashcards son best-effort)
- **TDD Cycle**: N/A (no es una implementación de código)

## Validation Commands

```bash
# Verificar directorio y archivos
ls -la logs/notebooklm/

# Verificar metadata JSON válido
python3 -c "import json; json.load(open('logs/notebooklm/metadata.json')); print('metadata.json: VALID')"

# Verificar mind-map JSON válido
python3 -c "import json; json.load(open('logs/notebooklm/mind-map.json')); print('mind-map.json: VALID')"

# Verificar que briefing-doc tiene contenido
test -s logs/notebooklm/briefing-doc.md && echo "briefing-doc.md: HAS CONTENT" || echo "briefing-doc.md: EMPTY/MISSING"

# Verificar que study-guide tiene contenido
test -s logs/notebooklm/study-guide.md && echo "study-guide.md: HAS CONTENT" || echo "study-guide.md: EMPTY/MISSING"

# Verificar que chat-responses tiene 3 secciones
grep -c "^## Pregunta" logs/notebooklm/chat-responses.md

# Contar total de archivos generados
ls logs/notebooklm/ | wc -l
```

## Notes

- **Rate Limiting**: Quiz y flashcards son "best-effort". Si Google rate-limita, el usuario puede generarlos manualmente desde la web UI de NotebookLM.
- **Idioma**: Reports se generan en español (`--language es`). Chat se hace en inglés para mejor calidad de respuesta de NotebookLM.
- **Tiempos estimados de procesamiento**:
  - Source indexing (YouTube): 30s - 10 min
  - Mind-map: instant
  - Reports (briefing, study-guide): 5-15 min cada uno
  - Quiz/Flashcards: 5-15 min cada uno (si no hay rate-limit)
  - Chat: instant
- **Autenticación**: Se requiere `notebooklm login` previo. Si la sesión expiró, el agente de setup lo detectará.
- **Dependencia**: `notebooklm-py` debe estar instalado (`pip install notebooklm-py`).
- **Contexto del video**: Este es el repo `the-library` de IndyDevDan (disler). Cristian lo clonó y lo usa como base para su propio sistema de distribución de skills.
