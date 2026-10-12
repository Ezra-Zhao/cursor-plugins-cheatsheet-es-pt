# 🔌 Plugins de Cursor — guía rápida (Español)

> Verificado con el README de [cursor/plugins](https://github.com/cursor/plugins) y el esquema oficial del manifiesto (2026-10-12).
> Qué es: un plugin empaqueta skills, reglas, comandos, agentes, hooks y servidores MCP en una sola unidad instalable para Cursor, declarada con un único manifiesto JSON.

---

## 1. Los bloques de un plugin

Un plugin es una carpeta. Cada bloque vive en su sitio:

| Bloque | Ubicación | Formato |
|---|---|---|
| Skills | `skills/<nombre>/SKILL.md` | Markdown con frontmatter `name` + `description` |
| Reglas | `rules/*.mdc` | Archivos `.mdc`, frontmatter: `description`, `alwaysApply`, `globs` opcional |
| Comandos | `commands/*.md` o `*.txt` | Comandos de barra (slash), frontmatter `name` + `description` |
| Agentes | `agents/*.md` | Definiciones de subagentes, frontmatter `name` + `description` |
| Hooks | `hooks/hooks.json` | Configuración de hooks del ciclo de vida |
| Servidores MCP | `mcp.json` en la raíz del plugin | Definiciones de servidores MCP |

Cursor descubre los componentes por carpeta. Solo declaras rutas en el manifiesto cuando mueves algo de su lugar por defecto.

---

## 2. El manifiesto: `.cursor-plugin/plugin.json`

Un archivo en la raíz del plugin: `.cursor-plugin/plugin.json`. El esquema oficial exige **un solo campo**: `name`.

Manifiesto mínimo válido:

```json
{
  "name": "mi-plugin"
}
```

- `name`: kebab-case en minúsculas — letras, dígitos y guiones (puntos permitidos), debe empezar y terminar con un carácter alfanumérico.
- Campos recomendados: `version` (semver), `description`, `author` (`{ "name": ..., "email": ... }`), `license`, `keywords`.
- Opcionales: `displayName`, `homepage`, `repository`, `logo`, `category`, `tags`, `minClientVersions`.
- Campos de componentes (`skills`, `rules`, `agents`, `commands`, `hooks`, `mcpServers`): ruta o glob, solo si no usas la ubicación por defecto.
- Toda ruta del manifiesto debe ser **relativa** — nada de rutas absolutas ni `..`.

Un manifiesto realista (modelado sobre el plugin oficial `teaching`):

```json
{
  "name": "teaching",
  "displayName": "Teaching",
  "version": "1.0.0",
  "description": "Skill mapping, practice plans, and learning retrospectives.",
  "author": { "name": "Cursor", "email": "plugins@cursor.com" },
  "license": "MIT",
  "keywords": ["teaching", "study-plan", "practice"],
  "category": "utilities",
  "skills": "./skills/"
}
```

---

## 3. Tu primer plugin en 3 pasos

**1. Escribe el manifiesto.** Crea la carpeta, mete `.cursor-plugin/plugin.json`, añade un `README.md` y un `LICENSE`, y agrega solo las carpetas de componentes que de verdad uses.

**2. Cárgalo en local.** Deja la carpeta en el directorio local de plugins — sin paso de instalación, Cursor la detecta directamente:

```
~/.cursor/plugins/local/<nombre-del-plugin>/
```

**3. Depura e itera.** Antes de darlo por listo, comprueba: el JSON se parsea, `name` es kebab-case válido, todas las rutas declaradas existen, y cada skill/regla/agente/comando lleva su frontmatter `name` + `description`. Atajo: el plugin oficial `create-plugin` genera la estructura y ejecuta estas comprobaciones por ti.

¿Varios plugins en un repo? Cada uno se registra con una entrada (`name` + `source`) en un `.cursor-plugin/marketplace.json` en la raíz.

---

## 4. Plugins oficiales que vale la pena estudiar

De la raíz del repo upstream (autor: Cursor, salvo indicación):

| Plugin | Qué hace |
|---|---|
| `create-plugin` | Genera y valida plugins nuevos — empieza por aquí |
| `cursor-sdk` | Crear apps y automatizaciones con el SDK de TypeScript |
| `advisor` | Consulta un modelo más potente antes de decisiones grandes |
| `orchestrate` | Reparte tareas grandes entre agentes en la nube en paralelo |
| `thermos` | Revisión profunda de ramas: auditorías de seguridad y corrección |
| `ralph-loop` | Bucles de IA iterativos y autorreferenciales |
| `agent-compatibility` | Escanea un repo buscando compatibilidad con agentes; audita arranque y docs |
| `teaching` | Mapas de skills, planes de práctica, retrospectivas de aprendizaje |

Decenas de integraciones más viven en `third_party/`: GitHub, Gmail, Google Drive, Playwright, Salesforce, HubSpot, Zoom, X y muchas otras. Lee una parecida a la tuya antes de escribir la tuya.

---

## Enlaces

- Repo upstream: https://github.com/cursor/plugins
- Esquema del manifiesto: https://github.com/cursor/plugins/blob/main/schemas/plugin.schema.json
- Esquema del marketplace: https://github.com/cursor/plugins/blob/main/schemas/marketplace.schema.json

---

*¿Un error? Abre un issue — la spec cambia a medida que Cursor publica novedades.*
