---
name: alusage-dev-workflow
description: >
  Ce skill doit être utilisé quand l'utilisateur veut "développer pour alusage",
  "créer une branche alusage", "travailler sur un client alusage",
  "déployer une branche alusage", "créer une PR GitHub alusage",
  "gérer les modules d'un client alusage", ou "utiliser le MCP jarvis".
version: 0.1.0
---

# Skill : Workflow de développement Alusage

## Architecture du projet

```
/home/njeudy/dev/Devops/odoo_jarvis_assistant/
├── clients/                    # Un dossier par client
│   ├── {client}/
│   │   ├── project_config.json # Config client (version, GitHub, Cloudron...)
│   │   ├── addons-store/       # Submodules OCA et modules externes
│   │   ├── docker/             # Images Docker client
│   │   ├── cloudron/           # Config déploiement Cloudron
│   │   └── .worktrees/         # Worktrees git par branche de dev
│   │       └── {branch}/       # ex: dev-20260312-1430/
├── mcp_server/                 # Serveur MCP (47 outils)
├── _bmad/                      # BMAD Method initialisé
├── CLAUDE.md                   # ⚠️ LIRE AVANT TOUT DEV
└── Makefile                    # Commandes make (référence, on préfère MCP)
```

Documentation technique complète :
`/home/njeudy/Nextcloud/Les Atelier du 97/docs/odoo_jarvis_assistant/`

## Clients disponibles

```bash
ls /home/njeudy/dev/Devops/odoo_jarvis_assistant/clients/
```
Actuellement : alusage16, alusage18, cem, evolusolar, juliennejavel, odoo10, odoo12, odoo15, odoo18, odoo19, reunion_portage, triboo

## Convention de branches de développement

**Format** : `dev-YYYYMMDD-HHMM`

```bash
# Générer un nom de branche
date "+dev-%Y%m%d-%H%M"   # ex: dev-20260312-1430
```

**Règles** :
- Une branche = une session de travail (pas une par ticket)
- Branches de production = `{version}` (ex: `18.0`, `19.0`) — JAMAIS `master` ou `main`
- `master` et `main` = branches de développement, PAS de production

## MCP Server Jarvis (outil principal)

Le MCP server `alusage-jarvis` est la **source de vérité unique** pour toutes les opérations.
Toujours préférer les outils MCP à `make` ou aux scripts bash directs.

### Outils MCP clés disponibles

| Outil MCP | Fonction |
|-----------|----------|
| `client` | Infos et gestion d'un client |
| `worktree` | Créer/lister/supprimer des worktrees |
| `branch` | Gestion des branches |
| `git` | Opérations git sur un client |
| `docker_branch` | Démarrer/arrêter une instance Docker par branche |
| `deployment` | Gestion des déploiements |
| `cloudron_tools` | Déploiement Cloudron (prod) |
| `module` | Gestion des modules Odoo |
| `system_git_config` | Configuration git système |

### Utilisation des outils MCP
```
# Via les outils MCP disponibles dans la session :
mcp__alusage-jarvis__worktree(action="create", client="{client}", branch="{branch}")
mcp__alusage-jarvis__docker_branch(action="start", client="{client}", branch="{branch}")
mcp__alusage-jarvis__git(action="status", client="{client}")
```

## GitHub — Pull Requests

- Org : `Alusage`
- Repo principal : `Alusage/odoo_alusage`
- CLI : `gh` (déjà authentifié)

```bash
# Créer une PR
gh pr create \
  --repo Alusage/odoo_alusage \
  --title "Dev {YYYYMMDD} {HHMM}" \
  --base {version_branche} \
  --head {dev-branch} \
  --body "..."

# Lister les PRs ouvertes
gh pr list --repo Alusage/odoo_alusage

# Voir le statut d'une PR
gh pr view {numero} --repo Alusage/odoo_alusage
```

## Règles d'architecture (OBLIGATOIRES)

**Avant tout développement sur le projet `odoo_jarvis_assistant`**, lire :
`/home/njeudy/dev/Devops/odoo_jarvis_assistant/CLAUDE.md`

Les 5 règles fondamentales :
1. **Dashboard STUPIDE** — pas de logique métier dans le dashboard
2. **MCP = Source de vérité** — toutes les données viennent du MCP
3. **Pas de duplication** — MCP ↔ Dashboard : une seule source
4. **Cloudron via MCP** — jamais d'appel direct Cloudron
5. **StandardResponse obligatoire** — format de réponse MCP uniforme

## Sources Odoo pour développement

Même base de sources que sudokeys-dev (partagée entre les deux contextes) :
```
/home/njeudy/dev/doc_technique/Odoo/
├── odoo/          # Community : odoo-14.0 à odoo-19.0 + master
├── enterprise/    # Enterprise : enterprise-14.0 à enterprise-18.0
├── oca/           # 57 repos OCA, chacun avec worktrees par version
└── externe/       # alusage-extra-addons, alusage-opl-addons, odoo-usability...
```

Versions clés selon les clients alusage :
- alusage18, odoo18 → `/home/njeudy/dev/doc_technique/Odoo/enterprise/enterprise-18.0/`
- alusage16, juliennejavel, triboo → `enterprise-16.0/`
- cem, odoo19 → `enterprise-19.0/` (ou `odoo-19.0/` pour community)
- odoo15 → `enterprise-15.0/`

Pour détecter la version d'un client :
```bash
python3 -c "import json; c=json.load(open('/home/njeudy/dev/Devops/odoo_jarvis_assistant/clients/{client}/project_config.json')); print(c.get('odoo_version','?'))"
```

OCA repos disponibles (pertinents pour alusage) :
```
/home/njeudy/dev/doc_technique/Odoo/oca/account-invoicing/account-invoicing-{version}/
/home/njeudy/dev/doc_technique/Odoo/oca/project/project-{version}/
/home/njeudy/dev/doc_technique/Odoo/oca/web/web-{version}/
/home/njeudy/dev/doc_technique/Odoo/oca/server-tools/server-tools-{version}/
```

Modules externes propres à alusage :
```
/home/njeudy/dev/doc_technique/Odoo/externe/alusage-extra-addons/
/home/njeudy/dev/doc_technique/Odoo/externe/alusage-opl-addons/
```

## Conventions de commit

Même standard Odoo que Sudokeys :
`[FIX]`, `[IMP]`, `[ADD]`, `[REM]`, `[REF]`, `[I18N]`

Format : `[TYPE] module: description courte`
