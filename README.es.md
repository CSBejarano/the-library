# The Library

Una meta-habilidad para la distribución privada de agénticos (habilidades, agentes y prompts) entre agentes, dispositivos y equipos.

![The Library](images/10_meta_skill.svg)

## Para Quién Es Esto

Si sos un ingeniero trabajando en más de 10 codebases con agentes y estás construyendo habilidades, agentes y prompts privados especializados — esto fue hecho para vos.

Si trabajás en uno o dos repos, no necesitás esto. Si instalás habilidades de internet público sin revisarlas, esto tampoco es para vos.

The Library resuelve un problema específico: construiste agénticos poderosos dispersos entre repos, dispositivos y equipos. Están duplicados, desincronizados y son difíciles de coordinar. Esto te da un catálogo de referencia único para distribuirlos de forma privada.

## Qué Es

The Library es una habilidad única cuyo único trabajo es gestionar otras habilidades. Es un catálogo de referencias — rutas de archivos locales y URLs de repos en GitHub — que apuntan a donde viven tus agénticos. Nada se copia ni se instala hasta que lo pedís.

Pensalo como un `package.json` para capacidades de agentes — pero en vez de paquetes, estás gestionando habilidades, agentes y prompts. En vez de un registro, estás apuntando a tus propios repos privados de GitHub y rutas locales.

**Esta es una aplicación puramente de agentes.** No hay scripts, no hay CLIs, no hay dependencias, no hay herramientas de build. La aplicación entera está codificada en `SKILL.md` y un conjunto de instrucciones de cookbook que le enseñan al agente exactamente qué hacer. El agente ES el runtime. Esto importa porque:

- Cualquier harness de agente que lea archivos de habilidades puede ejecutarlo (Claude Code, Pi, etc.)
- Podés modificar el comportamiento editando markdown, no código
- La habilidad puede ser extendida, forkeada y adaptada instantáneamente
- Un agente orquestador puede encadenar comandos de la biblioteca sin ninguna sobrecarga de herramientas

## Por Qué Existe

![El Problema: Dispersión de Habilidades](images/26_problem_skill_sprawl.svg)

A medida que construís con agentes de IA, acumulás habilidades, agentes personalizados y prompts — potencialmente cientos de ellos. Necesitás:

- **Reutilizarlos** entre proyectos sin copiar y pegar
- **Distribuirlos** a tus agentes corriendo en otros dispositivos (Mac mini, servidores remotos, sandboxes en la nube)
- **Compartirlos** con tu equipo sin hacer todo público
- **Mantenerlos privados** — son capacidades especializadas construidas para ventaja competitiva
- **Mantenerlos sincronizados** — una única fuente de verdad, no 10 copias desactualizadas

![El Problema: Equipos Aislados](images/32_problem_team_sharing.svg)

Las soluciones existentes no encajan:
- **`~/.claude/*` global** — expone todo a cada agente. Global es lo opuesto a especializado.
- **Plugins de Claude Code** — requiere infraestructura de marketplace, manifests, y te ata a una sola plataforma.
- **Monorepo único** — no refleja la realidad. Construís agénticos en codebases específicos para casos de uso específicos.

## Cómo Funciona

![La Solución: The Library](images/27_solution_library_workflow.svg)

### El Catálogo (`library.yaml`)

```yaml
default_dirs:
  skills:
    - default: .claude/skills/
    - global: ~/.claude/skills/
  agents:
    - default: .claude/agents/
    - global: ~/.claude/agents/
  prompts:
    - default: .claude/commands/
    - global: ~/.claude/commands/

library:
  skills:
    - name: my-skill
      description: What this skill does
      source: /Users/me/projects/tools/skills/my-skill/SKILL.md
      requires: [agent:helper-agent]
    - name: remote-skill
      description: A skill from a private repo
      source: https://github.com/myorg/private-skills/blob/main/skills/remote-skill/SKILL.md
  agents: []
  prompts: []
```

El catálogo almacena punteros, no copias. Las habilidades viven en sus repos de origen. Se traen bajo demanda.

### Formatos de Origen

| Formato            | Ejemplo                                                            |
| ------------------ | ------------------------------------------------------------------ |
| Sistema de archivos local | `/absolute/path/to/SKILL.md`                                |
| URL de navegador de GitHub | `https://github.com/org/repo/blob/main/path/to/SKILL.md`  |
| URL raw de GitHub  | `https://raw.githubusercontent.com/org/repo/main/path/to/SKILL.md` |

El origen apunta a un archivo específico. El sistema trae el directorio padre completo (las habilidades incluyen scripts, referencias, assets — no solo el archivo markdown).

Para repos privados, la autenticación usa claves SSH o `GITHUB_TOKEN` automáticamente.

### Dependencias Tipadas

Las dependencias usan referencias tipadas para evitar colisiones de nombres:

```yaml
requires: [skill:base-utils, agent:reviewer, prompt:task-router]
```

Las dependencias se resuelven y se traen primero, de forma recursiva.

## Prerrequisitos

- **Claude Code** (o un agente compatible que lea `.claude/skills/` — por ejemplo, Pi)
- **git** — para clonar fuentes y sincronizar el catálogo
- **gh** (opcional) — GitHub CLI para hacer forks, clonar y acceder a repos privados. Instalar: `brew install gh` o ver [documentación de gh](https://cli.github.com)
- **Clave SSH de GitHub o `GITHUB_TOKEN`** — para acceder a repos privados (no es necesario si usás `gh auth login`)
- **just** (opcional) — para atajos de justfile. Instalar: `brew install just` o ver [documentación de just](https://github.com/casey/just)

## Instalación

Este es un repo plantilla. Lo forkeás, lo clonás en tu directorio global de skills, y se convierte en un comando slash `/library` disponible en cada sesión de Claude Code.

### 1. Forkeá este Repo

Forkeá a tu propia cuenta de GitHub (se recomienda repo privado). Este fork es tu catálogo personal de biblioteca — vas a pushear las actualizaciones del catálogo ahí.

```bash
# Usando GitHub CLI
gh repo fork disler/the-library --private --clone=false
```

O forkeá manualmente desde la interfaz de GitHub.

### 2. Cloná en el Directorio Global de Skills

Cloná tu fork en `~/.claude/skills/library`. Esta ruta es lo que hace que `/library` esté disponible como comando slash global en Claude Code.

```bash
# Usando git
mkdir -p ~/.claude/skills/library
git clone <url-de-tu-fork> ~/.claude/skills/library

# O usando GitHub CLI
gh repo clone <tunombre>/the-library ~/.claude/skills/library
```

### 3. Configurá

Abrí `~/.claude/skills/library/SKILL.md` y actualizá la sección `## Variables` con la URL de tu fork. El agente lee estas variables en tiempo de ejecución para saber dónde sincronizar el catálogo.

```markdown
# Antes (valores por defecto de la plantilla)
- **LIBRARY_REPO_URL**: `<url de tu repo forkeado>`

# Después (tus valores)
- **LIBRARY_REPO_URL**: `https://github.com/tunombre/the-library.git`
```

Las otras dos variables (`LIBRARY_YAML_PATH` y `LIBRARY_SKILL_DIR`) son correctas por defecto si clonaste en `~/.claude/skills/library/`.

### 4. Verificá

Iniciá una nueva sesión de Claude Code en cualquier lugar. `/library list` debería funcionar y mostrar un catálogo vacío.

## Inicio Rápido

![Flujo de trabajo completo](images/45_solution_full_workflow.svg)

Este es el flujo de trabajo típico: **construir → catalogar → distribuir → usar**.

### Agregá un skill al catálogo

Construiste un skill de deploy en uno de tus repos. Registralo:

```
/library add deploy skill from https://github.com/yourorg/infra-tools/blob/main/skills/deploy/SKILL.md
```

Esto agrega una referencia a `library.yaml` y pushea la actualización a tu fork.

### Usalo en otro proyecto

En otro dispositivo, repo o agente:

```
/library use deploy
```

Esto descarga el skill desde el repo fuente a `.claude/skills/deploy/`.

¿Querés que esté disponible globalmente en esta máquina?

```
/library use deploy install globally
```

### Pusheá los cambios de vuelta

Mejoraste el skill localmente. Pusheá la actualización al repo fuente:

```
/library push deploy
```

Ahora cada dispositivo que ejecute `/library sync` obtiene la última versión.

### Sincronizá todo

Descargá la última versión de todos los elementos instalados:

```
/library sync
```

## Comandos

| Comando                     | Qué Hace                                                        |
| --------------------------- | --------------------------------------------------------------- |
| `/library install`          | Configuración inicial — fork, clon, configuración               |
| `/library add <details>`    | Registrar una nueva entrada en el catálogo                      |
| `/library use <name>`       | Traer desde el origen al directorio local (instalar o refrescar)|
| `/library push <name>`      | Enviar cambios locales de vuelta al origen                      |
| `/library remove <name>`    | Eliminar del catálogo y opcionalmente borrar la copia local     |
| `/library list`             | Mostrar el catálogo completo con estado de instalación          |
| `/library sync`             | Volver a traer todos los elementos instalados desde el origen   |
| `/library search <keyword>` | Buscar entradas por nombre o descripción                        |

### Atajos de Justfile

El `justfile` incluido te permite ejecutar comandos de library desde tu terminal sin una sesión interactiva de Claude.

```bash
just list                  # Listar catálogo
just use my-skill          # Traer un skill
just push my-skill         # Enviar cambios de vuelta
just add "name: foo, description: bar, source: /path/to/SKILL.md"
just sync                  # Volver a traer todos los elementos instalados
just search "keyword"
```

> **Nota:** Las recetas del Justfile usan `--dangerously-skip-permissions` porque el agente necesita acceso al sistema de archivos y a git para clonar, copiar y enviar. Revisá el `justfile` si querés modificar este comportamiento.

## Arquitectura

```
~/.claude/skills/library/     # El skill de The Library (instalado globalmente)
    SKILL.md                  # Instrucciones del agente — el cerebro
    library.yaml              # Tu catálogo de referencias
    cookbook/                  # Guías paso a paso para cada comando
        install.md
        add.md
        use.md
        push.md
        remove.md
        list.md
        sync.md
        search.md
    justfile                  # Atajos CLI para todos los comandos
    README.md                 # Este archivo
```

## Principios de Diseño

- **Privacidad primero**: Construido para tu agentic especializado y de ventaja competitiva. No es un marketplace público.
- **Basado en referencias**: El catálogo almacena punteros, no copias. Los skills viven en sus repositorios de origen.
- **Agente puro**: Sin scripts, sin herramientas de build. El SKILL.md le enseña al agente todo lo que necesita saber.
- **Agnóstico al agente**: El destino por defecto es `.claude/skills/` pero soporta cualquier directorio para cualquier harness de agente.
- **Catálogo, no manifiesto**: Las entradas definen lo que está disponible, no lo que está instalado. Se trae bajo demanda.

## El Stack Agentic

![El Stack Agentic](images/03_agentic_stack.svg)

| Capa            | Propósito                                               |
| --------------- | ------------------------------------------------------- |
| **Skills**      | Capacidades crudas — lo que un agente puede hacer       |
| **Agents**      | Escala + paralelismo + especialización                  |
| **Prompts**     | Orquestación — coordinar skills y agentes               |
| **Justfile**    | Acceso por terminal sin una sesión interactiva          |
| **The Library** | Distribución entre dispositivos, equipos y agentes      |

## Dominá la Programación Agentic
> Preparate para el futuro de la ingeniería de software

La Ingeniería Agentic es una NUEVA HABILIDAD para ingenieros de software. Y pronto será una habilidad requerida para ingenieros de software. Dominala antes que las masas con [Tactical Agentic Coding](https://agenticengineer.com/tactical-agentic-coding?y=tlibms)

Seguí el [canal de YouTube de IndyDevDan](https://www.youtube.com/@indydevdan) para mejorar tu ventaja en programación agentic.
