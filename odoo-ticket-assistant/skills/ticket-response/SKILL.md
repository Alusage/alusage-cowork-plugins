---
name: ticket-response
description: >
  Ce skill doit être utilisé quand l'utilisateur veut "rédiger une réponse à un ticket",
  "répondre à un client Odoo", "générer une réponse support", "envoyer une réponse helpdesk",
  "valider une réponse ticket", ou quand il s'agit de produire une communication client
  dans le contexte d'un ticket Odoo Helpdesk.
version: 0.1.0
---

# Skill : Rédaction de Réponses aux Tickets

## Rôle

Générer des réponses de qualité professionnelle aux tickets Odoo Helpdesk, calibrées selon le niveau de confiance disponible et adaptées au contexte client.

## Pipeline de réponse

### Étape 1 : Collecte du contexte

Avant de rédiger, toujours récupérer :

1. **Détails du ticket** via `mcp__odoo-alusage__get_record` (modèle `helpdesk.ticket`)
   - Champs : `name`, `description`, `partner_name`, `partner_email`, `stage_id`, `message_ids`, `tag_ids`

2. **Historique des échanges** via `mcp__odoo-alusage__search_records`
   - Modèle `mail.message`, filtre sur `res_id` = ID du ticket et `model` = `helpdesk.ticket`

3. **Contexte knowledge** : charger les fichiers pertinents dans `knowledge/`

### Étape 2 : Construction de la réponse

**Structure recommandée :**
```
[Accroche personnalisée au client]

[Corps de la réponse : explication / solution / étapes]

[Prochaine étape ou question de clarification si besoin]

[Signature professionnelle]
```

**Règles de rédaction :**
- Tutoiement ou vouvoiement selon le contexte client (vérifier historique)
- Langue du client (FR par défaut, détecter si EN ou autre)
- Ton : professionnel mais accessible, pas de jargon technique brut
- Longueur : 3-10 phrases selon la complexité
- Toujours conclure par une action claire (prochaine étape, confirmation)

### Étape 3 : Présentation selon le niveau de confiance

#### 🟢 AUTO (score ≥ 85)
Présenter la réponse prête à envoyer avec :
- Indicateur vert "Confiance élevée"
- Résumé de la source de confiance (résolution similaire #REF)
- Bouton d'action : "Envoyer tel quel" ou "Modifier avant envoi"

```markdown
✅ RÉPONSE PRÊTE À ENVOYER [Confiance: 92/100]
Source: Résolution similaire du [date] (#ancien-ticket)
---
[Corps de la réponse]
---
Souhaitez-vous envoyer cette réponse ou la modifier ?
```

#### 🟡 VALIDATION (score 60-84)
Présenter un brouillon avec :
- Indicateur jaune "Validation recommandée"
- Parties incertaines surlignées [À VÉRIFIER]
- Explication des doutes

```markdown
⚠️ BROUILLON - VALIDATION REQUISE [Confiance: 72/100]
Points à vérifier :
- [point 1 incertain]
- [point 2 à confirmer]
---
[Corps de la réponse avec [À VÉRIFIER] aux endroits incertains]
---
Veuillez vérifier les points marqués avant d'envoyer.
```

#### 🟠 PARTIEL (score 30-59)
Réponse partielle avec escalade :
- Indicateur orange "Contexte insuffisant"
- Partie répondable + partie à compléter
- Suggestion d'escalade

#### 🔴 ESCALADE (score < 30)
Pas de réponse automatique :
- Résumé du ticket pour handoff
- Questions clés à poser au client ou à l'expert

### Étape 4 : Post-envoi (apprentissage)

Après validation et envoi d'une réponse :
1. Proposer de sauvegarder la résolution dans `knowledge/resolutions/`
2. Format : `YYYY-MM-DD-#REF-titre-court.md`
3. Cette étape améliore le score de confiance pour les futurs tickets similaires

## Templates de réponse

Voir `references/templates-reponses.md` pour des templates par catégorie.

## Envoi via Odoo

Pour envoyer une réponse directement dans Odoo :
- Utiliser `mcp__odoo-alusage__create_record` sur `mail.message`
- Champs requis : `body`, `res_id` (ID du ticket), `model` = `helpdesk.ticket`, `message_type` = `comment`, `subtype_id` = 1 (pour réponse publique)
