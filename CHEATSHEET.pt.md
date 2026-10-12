# 🔌 Plugins do Cursor — guia rápido (Português)

> Verificado com o README de [cursor/plugins](https://github.com/cursor/plugins) e o schema oficial do manifesto (2026-10-12).
> O que é: um plugin empacota skills, regras, comandos, agentes, hooks e servidores MCP em uma única unidade instalável para o Cursor, declarada com um único manifesto JSON.

---

## 1. Os blocos de um plugin

Um plugin é uma pasta. Cada bloco fica no seu lugar:

| Bloco | Local | Formato |
|---|---|---|
| Skills | `skills/<nome>/SKILL.md` | Markdown com frontmatter `name` + `description` |
| Regras | `rules/*.mdc` | Arquivos `.mdc`, frontmatter: `description`, `alwaysApply`, `globs` opcional |
| Comandos | `commands/*.md` ou `*.txt` | Comandos de barra (slash), frontmatter `name` + `description` |
| Agentes | `agents/*.md` | Definições de subagentes, frontmatter `name` + `description` |
| Hooks | `hooks/hooks.json` | Configuração de hooks do ciclo de vida |
| Servidores MCP | `mcp.json` na raiz do plugin | Definições de servidores MCP |

O Cursor descobre os componentes pela pasta. Você só declara caminhos no manifesto quando move algo do lugar padrão.

---

## 2. O manifesto: `.cursor-plugin/plugin.json`

Um arquivo na raiz do plugin: `.cursor-plugin/plugin.json`. O schema oficial exige **um único campo**: `name`.

Manifesto mínimo válido:

```json
{
  "name": "meu-plugin"
}
```

- `name`: kebab-case em minúsculas — letras, dígitos e hífens (pontos permitidos), deve começar e terminar com caractere alfanumérico.
- Campos recomendados: `version` (semver), `description`, `author` (`{ "name": ..., "email": ... }`), `license`, `keywords`.
- Opcionais: `displayName`, `homepage`, `repository`, `logo`, `category`, `tags`, `minClientVersions`.
- Campos de componentes (`skills`, `rules`, `agents`, `commands`, `hooks`, `mcpServers`): caminho ou glob, só se você não usar o local padrão.
- Todo caminho no manifesto precisa ser **relativo** — nada de caminho absoluto nem `..`.

Um manifesto realista (modelado no plugin oficial `teaching`):

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

## 3. Seu primeiro plugin em 3 passos

**1. Escreva o manifesto.** Crie a pasta, coloque o `.cursor-plugin/plugin.json` dentro, adicione um `README.md` e um `LICENSE`, e inclua só as pastas de componentes que você realmente usa.

**2. Carregue localmente.** Ponha a pasta no diretório local de plugins — sem etapa de instalação, o Cursor reconhece direto:

```
~/.cursor/plugins/local/<nome-do-plugin>/
```

**3. Depure e repita.** Antes de dar como pronto, confira: o JSON é válido, o `name` é kebab-case correto, todos os caminhos declarados existem, e cada skill/regra/agente/comando tem o frontmatter `name` + `description`. Atalho: o plugin oficial `create-plugin` monta a estrutura e roda essas verificações por você.

Vários plugins num repo só? Cada um entra com uma entrada (`name` + `source`) num `.cursor-plugin/marketplace.json` na raiz.

---

## 4. Plugins oficiais que vale estudar

Da raiz do repo upstream (autor: Cursor, salvo indicação):

| Plugin | O que faz |
|---|---|
| `create-plugin` | Monta e valida plugins novos — comece por aqui |
| `cursor-sdk` | Criar apps e automações com o SDK de TypeScript |
| `advisor` | Consulta um modelo mais forte antes de decisões grandes |
| `orchestrate` | Distribui tarefas grandes entre agentes na nuvem em paralelo |
| `thermos` | Revisão profunda de branch: auditorias de segurança e correção |
| `ralph-loop` | Loops de IA iterativos e autorreferentes |
| `agent-compatibility` | Varre um repo em busca de compatibilidade com agentes; audita inicialização e docs |
| `teaching` | Mapeamento de skills, planos de prática, retrospectivas de aprendizado |

Dezenas de outras integrações ficam em `third_party/`: GitHub, Gmail, Google Drive, Playwright, Salesforce, HubSpot, Zoom, X e muitas outras. Leia uma parecida com a sua antes de escrever a sua.

---

## Links

- Repo upstream: https://github.com/cursor/plugins
- Schema do manifesto: https://github.com/cursor/plugins/blob/main/schemas/plugin.schema.json
- Schema do marketplace: https://github.com/cursor/plugins/blob/main/schemas/marketplace.schema.json

---

*Achou um erro? Abra uma issue — a spec muda conforme o Cursor lança novidades.*
