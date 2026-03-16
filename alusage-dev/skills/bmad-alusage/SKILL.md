---
name: bmad-alusage
description: >
  Ce skill doit être utilisé quand l'utilisateur veut "utiliser BMAD sur alusage",
  "créer une story alusage", "rédiger une spec technique pour odoo_jarvis_assistant",
  "planifier un dev alusage", "écrire un PRD pour jarvis", ou "documenter l'architecture alusage".
version: 0.1.0
---

# Skill : BMAD Method adapté Alusage / odoo_jarvis_assistant

## BMAD déjà initialisé

```
/home/njeudy/dev/Devops/odoo_jarvis_assistant/_bmad/
├── bmm/          # BM Manager
├── _config/      # Configuration BMAD
├── core/         # Core BMAD
└── _memory/      # Mémoire des agents
```

## Contrainte fondamentale

**Avant d'écrire n'importe quelle spec**, lire :
1. `CLAUDE.md` — règles d'architecture (5 règles guardien)
2. `/home/njeudy/Nextcloud/Les Atelier du 97/docs/odoo_jarvis_assistant/architecture/ARCHITECTURE_AGENT.md`
3. `mcp_server/core/responses.py` — format StandardResponse

Toute spec qui viole une des 5 règles doit être refusée et corrigée.

## Les 4 agents BMAD adaptés Alusage

### 1. Agent PM
**Entrée** : demande fonctionnelle ou ticket
**Sortie** : User Story (`docs/stories/{date}-{titre}.md`)

Rôle : définir le besoin en respectant l'architecture existante. Toujours cadrer par rapport aux 5 règles.

### 2. Agent Architect
**Entrée** : User Story
**Sortie** : Spec technique (`docs/architecture/{date}-spec-{titre}.md`)

Rôle : concevoir la solution technique en lisant :
- `mcp_server/core/server.py` — architecture du serveur
- `mcp_server/tools/base_tool.py` — classe de base des outils
- `mcp_server/core/responses.py` — StandardResponse
- Le tool existant le plus proche (pour respecter les patterns)
- La documentation Nextcloud si nécessaire

Contraintes Architect :
- Tout nouvel outil MCP hérite de `BaseTool`
- Toute réponse utilise `StandardResponse`
- Dashboard = jamais de logique, juste affichage des données MCP
- Cloudron = toujours via MCP, jamais en direct

### 3. Agent Dev
**Entrée** : Spec technique
**Sortie** : Code dans `mcp_server/tools/` ou client concerné

Rôle : implémenter en respectant les patterns existants. Toujours tester avec `make test-mcp`.

### 4. Agent QA
**Entrée** : Code + User Story
**Sortie** : Rapport de validation

Rôle : vérifier chaque critère d'acceptation, tester le MCP server, vérifier que le Dashboard parse correctement.

## Workflow complet

```
Demande / Ticket Odoo
      ↓
  [Agent PM]
  docs/stories/{date}-{titre}.md
      ↓  (valider que ça respecte les 5 règles)
  [Agent Architect]
  docs/architecture/{date}-spec-{titre}.md
      ↓
  git worktree add .worktrees/dev-YYYYMMDD-HHMM dev-YYYYMMDD-HHMM
      ↓
  [Agent Dev]
  Code dans mcp_server/tools/ ou clients/{client}/
  Commits [TYPE] module: desc
      ↓
  make test-mcp
      ↓
  [Agent QA]
      ↓
  gh pr create → Alusage/odoo_alusage
      ↓
  Fusionner vers {version_branche}
```

## Format des artefacts

### User Story
```markdown
# {Date} — {Titre}
Date: {DATE}
Règles respectées: #1 ✅ #2 ✅ #3 ✅ #4 ✅ #5 ✅

## Contexte
{Situation actuelle et problème}

## Besoin
En tant que {rôle}, je veux {action} afin de {bénéfice}.

## Critères d'acceptation
- [ ] CA1: {condition vérifiable via MCP}
- [ ] CA2: {le Dashboard affiche X depuis le MCP}

## Hors périmètre
- {Ce qu'on ne fait PAS}
```

### Spec Technique
```markdown
# Spec {Date} — {Titre}
Story: docs/stories/{date}-{titre}.md

## Conformité architecture
- [ ] Règle #1 (Dashboard stupide): {comment respecté}
- [ ] Règle #2 (MCP vérité): {point d'entrée MCP}
- [ ] Règle #5 (StandardResponse): {format retour}

## Outil(s) MCP à créer/modifier
| Fichier | Action | Description |
|---------|--------|-------------|
| tools/{nom}_tools.py | create/modify | {description} |

## Format StandardResponse
```python
return StandardResponse(
    success=True,
    data={...},
    message="..."
).dict()
```
```
