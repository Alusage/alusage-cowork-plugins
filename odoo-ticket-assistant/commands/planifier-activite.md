---
description: Créer une activité sur un ticket Odoo (type, échéance, assigné)
allowed-tools: mcp__odoo-alusage__search_records, mcp__odoo-alusage__get_record, mcp__odoo-alusage__create_record
argument-hint: "[#REF] [type: email|appel|reunion|todo] [echéance: J+X ou YYYY-MM-DD] [instance: sudokeys]"
---

Crée une activité planifiée sur un ticket Odoo Helpdesk avec type, date d'échéance et note.

## Instructions

1. **Parse les arguments** depuis "$ARGUMENTS" :
   - Numéro de ticket (REF ou ID)
   - Type d'activité : `email` → id=1, `appel` → id=2, `reunion` → id=3, `todo` → id=4
     Si non précisé → demander à l'utilisateur
   - Échéance : `J+X` (dans X jours), date littérale ou date YYYY-MM-DD
     Si non précisé → J+1 par défaut
   - Instance : sudokeys si mentionné

2. **Récupère le ticket** pour confirmer son existence et son titre

3. **Calcule la date d'échéance** en format YYYY-MM-DD :
   - "demain" / "J+1" → date du jour + 1
   - "cette semaine" → vendredi de la semaine courante
   - "J+X" → date du jour + X jours
   - date explicite → utiliser telle quelle

4. **Demande une note descriptive** si non fournie dans la requête :
   ```
   Pour quelle raison créez-vous cette activité ? (optionnel — appuyez Entrée pour passer)
   ```

5. **Crée l'activité** via `create_record` sur `mail.activity` :
   ```json
   {
     "res_model": "helpdesk.ticket",
     "res_id": [ID_DU_TICKET],
     "activity_type_id": [TYPE_ID],
     "date_deadline": "YYYY-MM-DD",
     "note": "[note optionnelle]",
     "summary": "[résumé court si fourni]"
   }
   ```

6. **Confirme la création** :
   ```
   ✅ Activité créée sur le ticket #REF
   Type : [Email/Appel/Réunion/To-Do]
   Échéance : [date lisible]
   Note : [note ou "(aucune)"]
   ```

## Résolution des types d'activités

**Toujours résoudre dynamiquement** — ne jamais hardcoder les IDs :

1. Lire `${CLAUDE_PLUGIN_ROOT}/config/odoo-schema.json` si disponible → utiliser `activity_types` de l'instance cible
2. Si le cache est absent ou vieux (> 30 jours) → faire une requête live `search_records` sur `mail.activity.type`
3. Matcher le nom demandé par l'utilisateur (fuzzy : "appel" → "Call", "réunion" → "Meeting", etc.)
4. Si ambiguïté → afficher la liste des types disponibles et laisser l'utilisateur choisir

Exemple de types typiques (peuvent varier selon l'instance) :
- Email, Appel/Call, Réunion/Meeting, To-Do, Upload Document
- + types custom créés par les équipes (toujours récupérer en live)

## Exemples d'usage

- `/planifier-activite 3332 appel J+2` → appel sur ticket #3332 dans 2 jours
- `/planifier-activite 3067 email 2026-03-20 Relancer le client` → email planifié au 20 mars
- `/planifier-activite 2977 todo demain` → tâche pour demain
