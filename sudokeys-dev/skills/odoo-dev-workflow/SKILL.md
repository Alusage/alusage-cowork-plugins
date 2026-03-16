---
name: odoo-dev-workflow
description: >
  Ce skill doit être utilisé quand l'utilisateur veut "développer un module Odoo",
  "créer une branche pour un ticket", "commencer un dev Sudokeys", "coder un fix Odoo",
  "ouvrir un module", "créer une MR", "démarrer une instance de test",
  ou quand il s'agit de tout workflow de développement Odoo dans le contexte Sudokeys.
version: 0.1.0
---

# Skill : Workflow de Développement Odoo Sudokeys

## Configuration workspace

Charger `${CLAUDE_PLUGIN_ROOT}/config/workspace.json` au début de chaque session dev pour obtenir :
- `workspace_root` : `/home/njeudy/dev/Sudokeys/workspace`
- `venv_activate` : `/home/njeudy/venv_3.12/bin/activate`
- `gitlab.api_url`, `gitlab.token_env`, `branch_prefix`, `commit_types`

## Structure d'un projet client

```
workspace/{client}/
├── docker-compose.yml          # Docker Odoo + PostgreSQL
├── odoo.conf                   # Config Odoo locale
├── .git/                       # Repo docker-odoo-local (infra)
└── odoo/
    └── addons-store/
        ├── {client}-addons/    # ← REPO PRINCIPAL (git)
        │   ├── .git/           # origin: gitlab.sudokeys.com/sudokeys/{client}-addons.git
        │   ├── {client}_base/
        │   ├── {client}_sale/
        │   ├── {client}_helpdesk/
        │   └── ...
        ├── odoo_entreprise/    # Modules enterprise
        ├── account-invoicing/  # OCA
        └── ...                 # Autres dépendances
```

## Convention de branches

**Format** : `TI{ticket_ref}` — ex: `TI3332` pour le ticket #3332

```bash
git checkout -b TI{REF}          # depuis master
git push -u origin TI{REF}       # premier push
```

**Branches permanentes** :
- `master` : branche principale, déployée en production
- `dev` : intégration continue (si utilisée)

## Convention de commits Odoo

Format : `[TYPE] {module}: {description courte}`

| Type | Usage | Exemple |
|------|-------|---------|
| `[FIX]` | Bug corrigé | `[FIX] aca_import: handle missing email field` |
| `[IMP]` | Amélioration | `[IMP] aca_sale: add discount field on order line` |
| `[ADD]` | Nouveau fichier/feature | `[ADD] aca_helpdesk: ticket automation rules` |
| `[REM]` | Suppression | `[REM] aca_base: remove deprecated wizard` |
| `[REF]` | Refactoring | `[REF] aca_project: split task model` |
| `[I18N]` | Traductions | `[I18N] aca_base: update fr translations` |

**Règles** :
- Module en snake_case : `aca_import`, `aca_sale`
- Description en anglais, infinitif, max 72 chars
- Corps de commit en français si contexte complexe

## Structure d'un module Odoo

```
{client}_{module}/
├── __manifest__.py             # Métadonnées
├── __init__.py
├── models/
│   ├── __init__.py
│   └── {model}.py
├── views/
│   └── {model}_views.xml
├── security/
│   └── ir.model.access.csv
├── data/                       # Données de configuration
├── wizard/                     # Wizards
├── report/                     # Rapports QWeb
├── static/
│   └── description/
│       └── icon.png
└── i18n/
    └── fr.po
```

## Commandes brainkeys

Toujours activer le venv avant d'utiliser brainkeys :
```bash
source /home/njeudy/venv_3.12/bin/activate
```

| Commande | Description |
|----------|-------------|
| `brainkeys start-odoo {client}` | Démarrer Odoo + PostgreSQL du client |
| `brainkeys stop-odoo {client}` | Arrêter les containers |
| `brainkeys odoo {client}` | Shell dans le container Odoo |
| `brainkeys pg {client}` | Shell PostgreSQL |
| `brainkeys datagraber {client}` | Télécharger dump de prod/backup-s3 |
| `brainkeys riplika {args}` | Gestion avancée des projets Odoo |

## Identifier le bon client depuis un ticket Odoo

Pour mapper un ticket Odoo à un dossier workspace :
1. `partner_company_name` ou `partner_name` → slug client (ex: "ACA" → `aca`)
2. Chercher dans `workspace/` un dossier dont le nom match (fuzzy)
3. En cas d'ambiguïté → lister les dossiers disponibles et demander

Clients disponibles : aca, atemis, bh, ecostab, emph, evolusolar, fpv, gazdom, gdc12, genergie, gpx, grc12, indus, petits, protex, provencelia, report18, scomo, securidom, tecsol, tevah, thermoceram

## GitLab API sans glab

Authentification : `GITLAB_TOKEN` dans l'environnement shell.

Créer une MR :
```bash
curl -s -X POST "https://gitlab.sudokeys.com/api/v4/projects/{project_id}/merge_requests" \
  -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "source_branch": "TI{REF}",
    "target_branch": "master",
    "title": "TI{REF}: {titre du ticket}",
    "description": "{corps de la MR}",
    "remove_source_branch": true
  }'
```

Trouver l'ID d'un projet :
```bash
curl -s "https://gitlab.sudokeys.com/api/v4/projects?search={client}-addons" \
  -H "PRIVATE-TOKEN: $GITLAB_TOKEN" | python3 -c "import json,sys; [print(p['id'], p['path_with_namespace']) for p in json.load(sys.stdin)]"
```

Voir `references/gitlab-api.md` pour plus d'opérations.
