---
name: ticket-triage
description: >
  Ce skill doit être utilisé quand l'utilisateur veut "analyser des tickets",
  "trier des tickets Odoo", "qualifier un ticket", "prioriser des demandes support",
  "voir les tickets ouverts", "triage helpdesk", ou quand il s'agit d'évaluer
  et catégoriser des tickets clients Odoo Helpdesk.
version: 0.1.0
---

# Skill : Triage et Qualification de Tickets Odoo

## Rôle

Analyser les tickets Odoo Helpdesk, les qualifier, les prioriser et déterminer une stratégie de réponse en fonction du niveau de confiance disponible.

## Processus de triage

### 1. Récupération des tickets

Utiliser le connecteur Odoo (`mcp__odoo-alusage__search_records`) avec le modèle `helpdesk.ticket`.

Filtres utiles :
- Tickets ouverts non résolus : `[['stage_id.name', 'not in', ['Résolu', 'Fermé', 'Closed', 'Done']]]`
- Par priorité : `[['priority', '=', '3']]` (0=normal, 1=low, 2=high, 3=urgent)
- Récents : `[['create_date', '>=', 'YYYY-MM-DD']]`

Champs clés à récupérer : `name`, `description`, `partner_name`, `partner_email`, `priority`, `stage_id`, `team_id`, `tag_ids`, `create_date`, `ticket_ref`

### 2. Calcul du score de confiance

Pour chaque ticket, calculer un score de confiance (0-100) basé sur :

| Facteur | Poids | Description |
|---------|-------|-------------|
| Correspondance résolutions passées | 40% | Ticket similaire déjà résolu dans `knowledge/resolutions/` |
| Contexte projet disponible | 30% | Doc projet pertinente dans `knowledge/projets/` |
| Contexte version disponible | 20% | Changelog correspondant dans `knowledge/versions/` |
| Contexte client connu | 10% | Fiche client dans `knowledge/clients/` |

### 3. Décision automatique selon le score

| Score | Action recommandée | Label |
|-------|--------------------|-------|
| 85-100 | Réponse automatique sans validation | 🟢 AUTO |
| 60-84 | Brouillon de réponse + validation humaine requise | 🟡 VALIDATION |
| 30-59 | Réponse partielle + escalade | 🟠 PARTIEL |
| 0-29 | Escalade manuelle uniquement | 🔴 ESCALADE |

### 4. Catégorisation

Catégories standard à identifier :
- **BUG** : dysfonctionnement avéré
- **QUESTION** : demande d'information ou d'explication
- **FEATURE** : demande d'évolution ou nouvelle fonctionnalité
- **CONFIG** : problème de configuration
- **FORMATION** : l'utilisateur ne sait pas utiliser une fonctionnalité
- **INTEGRATION** : problème lié à une connexion externe

### 5. Format de sortie du triage

Pour chaque ticket analysé, présenter :

```
[SCORE] [CATÉGORIE] #REF - Titre du ticket
Client: Nom (email)
Priorité: ★★★☆ (niveau)
Analyse: [résumé en 1-2 phrases]
Action: [AUTO/VALIDATION/PARTIEL/ESCALADE]
Justification confiance: [pourquoi ce score]
```

## Utilisation de la base de connaissance

Avant de scorer un ticket, toujours rechercher dans `knowledge/` via Read tool :

1. `knowledge/resolutions/` — chercher une résolution similaire (même fonctionnalité, même erreur, même client)
2. `knowledge/projets/` — charger la doc du projet concerné si identifiable
3. `knowledge/versions/` — vérifier si la version client est documentée
4. `knowledge/clients/` — charger la fiche client si disponible

## Connexion multi-instance

Ce plugin est configuré pour fonctionner avec deux instances Odoo :
- **Instance principale** (`odoo-alusage`) : instance connectée par défaut
- **Instance sudokeys** (`odoo-sudokeys`) : mentionner explicitement "sudokeys" pour cibler cette instance

Voir `references/multi-instance.md` pour les détails de configuration.
