---
description: Trier et qualifier les tickets Odoo ouverts
allowed-tools: mcp__odoo-alusage__search_records, mcp__odoo-alusage__get_record, Read, Glob
argument-hint: "[nombre] [priorité: urgent|high|all] [instance: sudokeys]"
---

Analyse les tickets Odoo Helpdesk ouverts et produit un rapport de triage qualifié.

## Instructions

1. **Détermine l'instance Odoo** à utiliser :
   - Si "$ARGUMENTS" contient "sudokeys" → utiliser les outils `mcp__odoo-sudokeys__*`
   - Sinon → utiliser les outils `mcp__odoo-alusage__*`

2. **Récupère les tickets ouverts** avec `search_records` sur `helpdesk.ticket` :
   - Filtre de base : exclure les tickets en stage final (Résolu/Fermé/Done)
   - Si "$ARGUMENTS" contient "urgent" : filtrer sur `priority = '3'`
   - Si "$ARGUMENTS" contient "high" : filtrer sur `priority in ['2','3']`
   - Nombre max : 20 tickets si non précisé, sinon le nombre indiqué dans "$ARGUMENTS"
   - Champs : `name`, `description`, `partner_name`, `partner_email`, `priority`, `stage_id`, `create_date`, `ticket_ref`, `tag_ids`

3. **Charge la base de connaissance** :
   - Liste les fichiers dans `${CLAUDE_PLUGIN_ROOT}/knowledge/resolutions/`
   - Liste les fichiers dans `${CLAUDE_PLUGIN_ROOT}/knowledge/clients/`
   - Liste les fichiers dans `${CLAUDE_PLUGIN_ROOT}/knowledge/projets/`

4. **Pour chaque ticket**, applique le skill `ticket-triage` :
   - Calcule le score de confiance (0-100)
   - Identifie la catégorie (BUG/QUESTION/CONFIG/FEATURE/INTEGRATION)
   - Détermine l'action recommandée (AUTO/VALIDATION/PARTIEL/ESCALADE)

5. **Produit le rapport de triage** :

```
# Rapport de triage — [DATE]
Instance: [principale/sudokeys]
Tickets analysés: [N] | AUTO: [N] 🟢 | VALIDATION: [N] 🟡 | ESCALADE: [N] 🔴

## Tickets à traiter en priorité

### 🟢 Réponse automatique possible
[liste des tickets avec score ≥ 85]

### 🟡 Validation requise avant envoi
[liste des tickets avec score 60-84]

### 🔴 Escalade manuelle
[liste des tickets avec score < 60]

## Statistiques
- Score confiance moyen : [X]/100
- Catégorie la plus fréquente : [catégorie]
- Plus ancien ticket sans réponse : #REF (X jours)
```

6. **Propose les actions suivantes** :
   - "Utilise `/repondre-ticket [REF]` pour traiter un ticket spécifique"
   - "Utilise `/traiter-auto` pour envoyer toutes les réponses AUTO en une fois"
