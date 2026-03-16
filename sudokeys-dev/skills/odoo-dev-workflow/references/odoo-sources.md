# Référence : Sources Odoo disponibles

## Localisation

```
/home/njeudy/dev/doc_technique/Odoo/
├── odoo/               # Core Odoo (Community)
│   ├── odoo-14.0/      # worktree git version 14
│   ├── odoo-15.0/
│   ├── odoo-16.0/      # ← ACA, ...
│   ├── odoo-17.0/
│   ├── odoo-18.0/
│   ├── odoo-19.0/
│   └── odoo-master/
├── enterprise/         # Odoo Enterprise
│   ├── enterprise-14.0/
│   ├── enterprise-15.0/
│   ├── enterprise-16.0/
│   ├── enterprise-17.0/
│   └── enterprise-18.0/
├── oca/                # 57 repos OCA (chacun avec worktrees par version)
│   ├── account-invoicing/
│   │   ├── account-invoicing-15.0/
│   │   ├── account-invoicing-16.0/
│   │   └── ...
│   ├── project/
│   ├── web/
│   ├── server-tools/
│   ├── server-ux/
│   ├── partner-contact/
│   ├── sale-workflow/
│   ├── stock-logistics-workflow/
│   └── ... (57 au total)
├── externe/            # Dépots externes utilisés
│   ├── alusage-extra-addons/
│   ├── odoo-usability/
│   ├── besacraft-addons/
│   └── ...
└── docs_md/            # Documentation générée (RAG)
```

## Version Odoo par client Sudokeys

Toujours vérifier dans le README du repo client :
```bash
grep -i "version\|odoo" /home/njeudy/dev/Sudokeys/workspace/{client}/README.md 2>/dev/null | head -5
# Ou regarder le badge CI dans le README
```

Versions connues :
- **aca** : 16.0 (confirmé badge CI)
- Autres : à détecter via README ou manifest.json du module

## Chemins d'accès rapide (version 16.0)

### Core Odoo 16
```
BASE: /home/njeudy/dev/doc_technique/Odoo/odoo/odoo-16.0/addons/
```
Modules utiles :
- `helpdesk/` → modèles tickets (dans Enterprise uniquement)
- `sale/`, `sale_management/` → ventes
- `project/` → projets
- `account/` → comptabilité
- `stock/` → inventaire
- `mail/` → système de messagerie
- `base/` → res.partner, res.company, etc.

### Enterprise 16
```
BASE: /home/njeudy/dev/doc_technique/Odoo/enterprise/enterprise-16.0/
```
Contient : `helpdesk/`, `timesheet_grid/`, `account_accountant/`, etc.

### OCA 16.0 — repos les plus pertinents
```
BASE: /home/njeudy/dev/doc_technique/Odoo/oca/{repo}/{repo}-16.0/
```
| Repo OCA | Contenu |
|----------|---------|
| `account-invoicing` | Facturation avancée |
| `account-financial-tools` | Outils comptables |
| `project` | Modules projet |
| `sale-workflow` | Workflow ventes |
| `server-tools` | Outils serveur (sequences, base_setup...) |
| `server-ux` | UX améliorée |
| `web` | Composants web |
| `partner-contact` | Contacts avancés |
| `stock-logistics-workflow` | Workflow logistique |

## Utilisation par l'Agent Architect

### Pattern de recherche

1. **Identifier le module Odoo natif concerné** :
```bash
ls /home/njeudy/dev/doc_technique/Odoo/enterprise/enterprise-16.0/ | grep {mot_cle}
ls /home/njeudy/dev/doc_technique/Odoo/odoo/odoo-16.0/addons/ | grep {mot_cle}
```

2. **Lire le modèle principal** :
```bash
# Exemple : modèle helpdesk.ticket
cat /home/njeudy/dev/doc_technique/Odoo/enterprise/enterprise-16.0/helpdesk/models/helpdesk_ticket.py
```

3. **Lire les vues existantes** :
```bash
cat /home/njeudy/dev/doc_technique/Odoo/enterprise/enterprise-16.0/helpdesk/views/helpdesk_ticket_views.xml
```

4. **Chercher des patterns OCA similaires** :
```bash
grep -r "champ_recherché" /home/njeudy/dev/doc_technique/Odoo/oca/{repo}/{repo}-16.0/ --include="*.py" -l
```

5. **Comparer avec code client** :
```bash
cat /home/njeudy/dev/Sudokeys/workspace/{client}/odoo/addons-store/{client}-addons/{client}_{module}/models/{model}.py
```

## Script de détection de version

```bash
# Détecter la version Odoo d'un client
detect_version() {
    local client=$1
    local workspace="/home/njeudy/dev/Sudokeys/workspace/${client}"

    # Chercher dans les manifests des modules
    version=$(grep -r "\"version\"" "${workspace}/odoo/addons-store/${client}-addons" \
        --include="__manifest__.py" -m 1 | grep -oP '"\d+\.\d+\.' | head -1 | tr -d '".')

    # Fallback : README
    if [ -z "$version" ]; then
        version=$(grep -i "odoo" "${workspace}/README.md" 2>/dev/null | grep -oP '\d{2}\.\d' | head -1)
    fi

    echo "${version:-unknown}"
}
```

## Bonnes pratiques de consultation

- **Lire les modèles natifs** avant de créer un champ (éviter les doublons)
- **Vérifier les contraintes SQL** existantes (`_sql_constraints`)
- **Regarder les méthodes override** déjà en place dans le module client
- **Consulter les OCA** pour des patterns d'implémentation éprouvés
- **Ne pas copier** le code enterprise (licence), s'en inspirer uniquement
