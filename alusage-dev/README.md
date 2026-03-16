# Plugin alusage-dev

Workflow de développement Alusage : gestion des clients Odoo, branches de dev, GitHub PRs, instances Docker, et développement du projet `odoo_jarvis_assistant` lui-même.

## Deux contextes de travail

### 1. Développement de `odoo_jarvis_assistant`
Le projet racine (`/home/njeudy/dev/Devops/odoo_jarvis_assistant/`) est le générateur de dépôts clients Odoo. BMAD est déjà initialisé (`_bmad/`).
- Architecture guardian : 5 règles à respecter (`CLAUDE.md`)
- MCP server à enrichir quand nécessaire
- Documentation dans Nextcloud

### 2. Travail sur les projets clients
Les clients sont dans `clients/{client}/`. Chaque client a son propre dépôt Git, sa config `project_config.json`, et ses worktrees de branches.

## Commandes disponibles

| Commande | Description |
|----------|-------------|
| `/dev-alusage [client]` | Démarrer une session de dev : créer branche `dev-YYYYMMDD-HHMM` + worktree |
| `/create-pr-alusage [client]` | Créer une PR GitHub depuis la config du client |
| `/deploy-branch [client] [start\|stop\|status]` | Gérer les instances Docker via MCP Jarvis |
| `/dev-status-alusage [client]` | Vue d'ensemble : worktrees, instances, PRs |

## MCP Server Jarvis

Le serveur MCP `alusage-jarvis` est connecté et expose 47 outils.
Il est la **source de vérité unique** pour toutes les opérations (pas de `make` direct).

## Convention de branches

- Dev : `dev-YYYYMMDD-HHMM`
- Production : `{version}` (18.0, 19.0...) — jamais `master`
