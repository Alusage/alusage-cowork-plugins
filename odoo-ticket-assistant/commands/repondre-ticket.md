---
description: Générer et envoyer une réponse à un ticket Odoo
allowed-tools: mcp__odoo-alusage__get_record, mcp__odoo-alusage__search_records, mcp__odoo-alusage__create_record, mcp__odoo-alusage__update_record, Read, Glob, Write, Bash(date:*), Bash(curl:*)
argument-hint: "[#REF ou ID du ticket] [template: accusé|résolu|notation|custom] [note-interne] [instance: sudokeys]"
---

Génère une réponse qualifiée pour le ticket Odoo spécifié et propose de l'envoyer selon le niveau de confiance.

## Résolution des templates Odoo

**Toujours résoudre dynamiquement** — ne jamais hardcoder les IDs :

1. Lire `${CLAUDE_PLUGIN_ROOT}/config/odoo-schema.json` si disponible → utiliser `mail_templates` de l'instance cible
2. Si cache absent → faire une requête live `search_records` sur `mail.template` avec `[["model","=","helpdesk.ticket"]]`
3. Présenter la liste des templates disponibles et choisir le plus adapté au contexte
4. Les IDs varient selon l'instance — toujours les résoudre depuis le cache ou live

## Instructions

1. **Détermine l'instance et l'identifiant du ticket** depuis "$ARGUMENTS" :
   - Si contient "sudokeys" → instance sudokeys
   - Extraire le numéro de ticket (REF ou ID numérique)
   - Si contient "note-interne" → la réponse sera un log note (non visible par le client)
   - Si contient un nom de template → utiliser ce template Odoo comme base

2. **Récupère le ticket complet** via `get_record` sur `helpdesk.ticket` avec les champs :
   `name`, `description`, `partner_name`, `partner_email`, `partner_phone`, `priority`, `stage_id`, `create_date`, `ticket_ref`, `tag_ids`, `message_ids`

3. **Récupère l'historique des échanges** via `search_records` sur `mail.message` :
   - Filtre : `[['res_id', '=', ID_TICKET], ['model', '=', 'helpdesk.ticket']]`
   - Champs : `body`, `author_id`, `date`, `message_type`, `subtype_id`

4. **Charge le contexte pertinent** depuis `${CLAUDE_PLUGIN_ROOT}/knowledge/` :
   - Chercher dans `resolutions/` : résolutions similaires par mots clés
   - Charger la fiche client depuis `clients/` si disponible
   - Charger la doc projet depuis `projets/` si identifiable
   - Charger le changelog depuis `versions/` si pertinent

5. **Calcule le score de confiance** (0-100) selon la grille du skill `ticket-triage`

6. **Choix du mode de réponse** — 3 options :

   **Option A : Template Odoo** (si ticket simple ou accusé/clôture)
   - Utiliser directement un template existant (#46, #47, #48)
   - Personnaliser les variables `{{ object.name }}`, `{{ partner.name }}` etc.
   - Indiquer quel template sera utilisé

   **Option B : Réponse générée sur mesure** (cas complexe ou custom)
   - Appliquer le skill `ticket-response` pour rédiger
   - Utiliser les templates de `skills/ticket-response/references/templates-reponses.md` comme base
   - Personnaliser avec le contexte client et les résolutions connues

   **Option C : Note interne** (log note non envoyée au client)
   - Rédigée pour l'équipe support interne
   - Visible uniquement dans le chatter Odoo, pas envoyée au client

7. **Présente la réponse avec l'indicateur de confiance** :

   **Si score ≥ 85 (AUTO)** :
   ```
   ✅ RÉPONSE PRÊTE — Confiance : [score]/100
   Mode : [Template Odoo #XX / Réponse personnalisée / Note interne]
   Source confiance : [résolution de référence]

   ---
   [Corps de la réponse]
   ---

   → Envoyer directement ? (oui / modifier / envoyer comme note interne)
   ```

   **Si score 60-84 (VALIDATION)** :
   ```
   ⚠️ BROUILLON — Validation requise — Confiance : [score]/100
   Points incertains : [liste]

   ---
   [Corps de la réponse avec [À VÉRIFIER] aux endroits incertains]
   ---

   → Valider et envoyer ? (oui / modifier / escalader)
   ```

   **Si score < 60** :
   ```
   🔴 ESCALADE RECOMMANDÉE — Confiance : [score]/100
   Raison : [manque de contexte spécifique]

   [Brouillon partiel si possible]

   → Que souhaitez-vous faire ? (modifier / escalader / abandonner)
   ```

8. **Si l'utilisateur confirme l'envoi** :

   **Pour un message client (réponse publique)** :
   - `create_record` sur `mail.message` avec :
     ```json
     {
       "model": "helpdesk.ticket",
       "res_id": [ID_TICKET],
       "body": "[corps HTML de la réponse]",
       "message_type": "comment",
       "subtype_id": 1
     }
     ```

   **Pour une note interne** :
   - Même structure mais `subtype_id: 2`

   - Confirmer avec le lien Odoo vers le ticket

9. **Notifications post-envoi** (si configurées) :
   - Si Slack connecté ET instance sudokeys → poster un résumé dans le canal configuré
   - Si Discord webhook configuré AND instance alusage → POST vers l'URL webhook
   - Voir `${CLAUDE_PLUGIN_ROOT}/config/notifications.json` pour la configuration

10. **Après envoi réussi**, proposer :
    - Sauvegarder la résolution dans `knowledge/resolutions/`
    - Planifier une activité de suivi (`/planifier-activite [REF] todo J+3`)
    - Passer le ticket au statut "Résolu" si approprié
