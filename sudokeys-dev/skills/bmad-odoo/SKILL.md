---
name: bmad-odoo
description: >
  Ce skill doit être utilisé quand l'utilisateur veut "utiliser BMAD", "créer une story",
  "rédiger une spec technique", "initialiser BMAD sur un projet", "générer un PRD",
  "documenter l'architecture d'un dev", "créer un plan d'implémentation Odoo",
  ou quand il faut structurer un développement Odoo avec la méthodologie BMAD.
version: 0.1.0
---

# Skill : BMAD Method adapté Odoo Sudokeys

## Qu'est-ce que BMAD ?

BMad Method est une méthodologie de développement AI-native qui structure le travail entre plusieurs agents spécialisés. Appliquée à Odoo Sudokeys, elle transforme un ticket client en développement complet via une séquence d'artefacts.

**Initialiser BMAD dans un projet** :
```bash
cd /home/njeudy/dev/Sudokeys/workspace/{client}/odoo/addons-store/{client}-addons
npx bmad-method@latest
```

## Les 4 agents BMAD adaptés Odoo

### 1. Agent PM (Product Manager)
**Entrée** : ticket Odoo brut
**Sortie** : User Story structurée (`docs/stories/TI{REF}-{titre}.md`)

Rôle : transformer la demande client en spécification fonctionnelle claire, avec critères d'acceptation vérifiables.

### 2. Agent Architect
**Entrée** : User Story
**Sortie** : Spec technique (`docs/architecture/TI{REF}-spec.md`)

Rôle : identifier les modèles Odoo impactés, les fichiers à créer/modifier, les dépendances, les impacts sur les autres modules du client.

**Sources à consulter** (voir `references/odoo-sources.md` pour les chemins complets) :
1. Module natif Odoo (`/home/njeudy/dev/doc_technique/Odoo/odoo/odoo-{VERSION}/addons/{module}/`)
2. Module enterprise si applicable (`/home/njeudy/dev/doc_technique/Odoo/enterprise/enterprise-{VERSION}/{module}/`)
3. Module(s) OCA pertinents (`/home/njeudy/dev/doc_technique/Odoo/oca/{repo}/{repo}-{VERSION}/`)
4. Code existant du module client (`{workspace_root}/{client}/odoo/addons-store/{client}-addons/`)

### 3. Agent Dev
**Entrée** : Spec technique + code existant du module
**Sortie** : Code implémenté, commits `[TYPE] module: description`

Rôle : implémenter le code en respectant les conventions Odoo et Sudokeys, module par module.

### 4. Agent QA
**Entrée** : Code implémenté + User Story
**Sortie** : Rapport de validation + checklist de tests

Rôle : vérifier que chaque critère d'acceptation est couvert, signaler les régressions potentielles.

## Workflow complet d'un ticket vers une MR

```
Ticket Odoo #REF
      ↓
  [Agent PM]
  docs/stories/TI{REF}-{titre}.md
      ↓
  [Agent Architect]
  docs/architecture/TI{REF}-spec.md
      ↓
  git checkout -b TI{REF}
      ↓
  [Agent Dev]
  Code dans {client}-addons/
  commits: [TYPE] module: desc
      ↓
  [Agent QA]
  Checklist de validation
      ↓
  MR GitLab: TI{REF} → master
      ↓
  Réponse sur ticket Odoo + activité de suivi
```

## Format des artefacts

### User Story (PM)

```markdown
# TI{REF} — {Titre de la story}
Date: {DATE}
Ticket Odoo: #{REF}
Client: {nom}
Priorité: {priorité du ticket}

## Contexte
{Qui est l'utilisateur, quel est son contexte}

## Besoin
En tant que {rôle}, je veux {action} afin de {bénéfice}.

## Critères d'acceptation
- [ ] CA1: {condition vérifiable}
- [ ] CA2: {condition vérifiable}
- [ ] CA3: {condition vérifiable}

## Hors périmètre
- {ce qu'on ne fait PAS dans ce ticket}

## Notes techniques
{Contraintes, dépendances, modules Odoo concernés}
```

### Spec technique (Architect)

```markdown
# Spec TI{REF} — {Titre}
Date: {DATE}
Story: docs/stories/TI{REF}-{titre}.md

## Analyse de l'existant
{Modules et fichiers existants impactés, avec chemin}

## Modèles Odoo impactés
| Modèle | Action | Champs ajoutés/modifiés |
|--------|--------|------------------------|
| {model} | modify | champ1 (Char), champ2 (Many2one) |

## Fichiers à créer/modifier
| Fichier | Action | Description |
|---------|--------|-------------|
| models/{model}.py | modify | Ajouter champ X |
| views/{model}_views.xml | modify | Afficher champ X |
| security/ir.model.access.csv | modify | Droits lecture |

## Points d'attention
- {risque de régression 1}
- {compatibilité version Odoo}

## Séquence d'implémentation
1. {étape 1 : modifier le modèle}
2. {étape 2 : créer/modifier la vue}
3. {étape 3 : migration si nécessaire}
4. {étape 4 : tests}
```

## Règles de conduite des agents

**Agent PM** :
- Reformuler sans jargon technique
- Les critères d'acceptation doivent être testables humainement (pas "ça marche" mais "le champ X est visible dans le formulaire Y")
- Toujours préciser le hors-périmètre

**Agent Architect** :
- **Toujours consulter les sources Odoo natives** avant de spécifier : lire le modèle Odoo standard, identifier les champs existants, les méthodes override possibles
- Vérifier si l'OCA propose déjà un module répondant au besoin (éviter de réinventer)
- Lire le code existant du module client avant de spécifier (`Read` sur les modèles concernés)
- Préférer modifier l'existant à créer un nouveau module sauf si isolement nécessaire
- Anticiper les migrations de données si modification de champs existants
- Détecter la version Odoo du client (voir `references/odoo-sources.md`) pour pointer les bons worktrees

**Agent Dev** :
- Un commit par sous-tâche logique, pas un commit global
- Tester l'upgrade du module dans brainkeys avant de committer
- Respecter strictement les conventions de commit `[TYPE] module: desc`

**Agent QA** :
- Vérifier chaque critère d'acceptation de la story
- Signaler explicitement tout risque de régression sur d'autres modules
- La checklist doit être reproductible par quelqu'un qui ne connaît pas le code

## Dossier docs dans le repo

Créer si absent :
```
{client}-addons/
└── docs/
    ├── stories/        # User stories par ticket
    ├── architecture/   # Specs techniques
    └── decisions/      # ADR (Architecture Decision Records) si pertinent
```

Ces fichiers sont versionnés dans git → traçabilité complète ticket → code.
