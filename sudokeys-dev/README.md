# Sudokeys Dev Plugin

Workflow de développement Odoo structuré pour Sudokeys : de la branche TI à la MR GitLab, avec méthodologie BMAD intégrée.

## Vue d'ensemble

Ce plugin connecte les tickets Odoo Helpdesk (via `odoo-ticket-assistant`) au cycle de développement complet :

```
Ticket Odoo → Story (PM) → Spec (Architect) → Code (Dev) → QA → MR GitLab
```

## Commandes

| Commande | Description | Exemple |
|----------|-------------|---------|
| `/dev-ticket` | Workflow BMAD complet depuis un ticket | `/dev-ticket 3332 client aca` |
| `/start-instance` | Démarrer Odoo local (brainkeys) | `/start-instance aca` |
| `/stop-instance` | Arrêter Odoo local | `/stop-instance aca` |
| `/create-mr` | Créer une MR GitLab | `/create-mr 3332 aca --draft` |
| `/dev-status` | Vue d'ensemble des branches TI actives | `/dev-status --all` |

## Skills inclus

- **odoo-dev-workflow** : conventions Sudokeys, structure modules, brainkeys, API GitLab
- **bmad-odoo** : méthodologie BMAD (agents PM / Architect / Dev / QA) adaptée Odoo

## Prérequis

**GITLAB_TOKEN** : exporter dans le shell avant d'utiliser `/create-mr` :
```bash
export GITLAB_TOKEN=votre-token-gitlab
# Ou ajouter dans ~/.bashrc pour persistance
```

**brainkeys** : dans le venv `/home/njeudy/venv_3.12/` — activé automatiquement par les commandes.

**npx** : pour initialiser BMAD dans un projet :
```bash
cd /home/njeudy/dev/Sudokeys/workspace/{client}/odoo/addons-store/{client}-addons
npx bmad-method@latest
```

## Intégration avec odoo-ticket-assistant

Ce plugin est le complément dev du plugin `odoo-ticket-assistant`. Workflow typique :

1. `/analyser-tickets` → triage du helpdesk
2. Ticket FEATURE ou BUG-dev → `/dev-ticket {REF}`
3. Dev implémenté → `/start-instance {client}` pour tester
4. Tests OK → `/create-mr {REF}`
5. MR mergée → `/repondre-ticket {REF}` pour informer le client

## Structure workspace

```
/home/njeudy/dev/Sudokeys/workspace/
└── {client}/                          # 24 clients
    ├── docker-compose.yml
    └── odoo/
        └── addons-store/
            └── {client}-addons/       # Repo principal GitLab
                ├── .git/              # origin: gitlab.sudokeys.com/sudokeys/{client}-addons
                ├── {client}_base/
                ├── {client}_*.../
                └── docs/              # Créé par BMAD
                    ├── stories/       # User Stories TI{REF}
                    └── architecture/  # Specs techniques TI{REF}
```

## GitLab self-hosted

- Host : `gitlab.sudokeys.com:10022`
- Namespace projets : `sudokeys/`
- Convention branches : `TI{ticket_ref}` (ex: `TI3332`)
- Convention commits : `[TYPE] module: description` (Odoo standard)
