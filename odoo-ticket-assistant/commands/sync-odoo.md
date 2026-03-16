---
description: Synchroniser les métadonnées Odoo (templates, activités, stages, équipes)
allowed-tools: mcp__odoo-alusage__search_records, Read, Write, Bash(date:*)
argument-hint: "[instance: sudokeys|principale|toutes]"
---

Interroge l'instance Odoo en live et met à jour le cache local des métadonnées dans `config/odoo-schema.json`.
À lancer après chaque mise à jour Odoo ou quand de nouveaux templates/types sont créés.

## Instructions

1. **Détermine les instances à synchroniser** depuis "$ARGUMENTS" :
   - `sudokeys` → synchroniser uniquement sudokeys
   - `principale` ou vide → synchroniser uniquement l'instance principale
   - `toutes` → synchroniser les deux

2. **Pour chaque instance à synchroniser**, effectue les requêtes suivantes en parallèle :

   **Templates mail helpdesk** — `mail.template` :
   ```
   domain: [["model", "=", "helpdesk.ticket"]]
   fields: ["id", "name", "subject", "model"]
   ```

   **Types d'activité** — `mail.activity.type` :
   ```
   domain: []
   fields: ["id", "name", "icon", "category", "res_model", "sequence"]
   order: "sequence asc"
   ```

   **Stages des tickets** — `helpdesk.stage` :
   ```
   domain: []
   fields: ["id", "name", "sequence", "team_ids", "fold"]
   order: "sequence asc"
   ```

   **Équipes helpdesk** — `helpdesk.team` :
   ```
   domain: []
   fields: ["id", "name", "alias_email", "member_ids"]
   ```

   **Tags helpdesk** — `helpdesk.ticket.tag` :
   ```
   domain: []
   fields: ["id", "name"]
   ```

3. **Construit le fichier de cache** `${CLAUDE_PLUGIN_ROOT}/config/odoo-schema.json` :

```json
{
  "_meta": {
    "last_sync": "[TIMESTAMP ISO]",
    "instances": ["principale", "sudokeys"]
  },
  "principale": {
    "mail_templates": [
      { "id": 46, "name": "Ticket: Reception Acknowledgment", "subject": "..." },
      ...
    ],
    "activity_types": [
      { "id": 1, "name": "Email", "icon": "fa-envelope", "category": "default" },
      { "id": 2, "name": "Call", "icon": "fa-phone", "category": "phonecall" },
      ...
    ],
    "stages": [
      { "id": 1, "name": "Nouveau", "sequence": 1, "fold": false },
      ...
    ],
    "teams": [
      { "id": 1, "name": "Support", ... },
      ...
    ],
    "tags": [
      { "id": 1, "name": "bug" },
      ...
    ]
  },
  "sudokeys": {
    "...": "idem"
  }
}
```

4. **Écrit le fichier** via Write tool sur `${CLAUDE_PLUGIN_ROOT}/config/odoo-schema.json`

5. **Affiche un résumé de la synchronisation** :
```
✅ Synchronisation Odoo terminée — [TIMESTAMP]

Instance principale :
  • Templates mail helpdesk : [N] (ex: "Reception Acknowledgment", "Ticket Solved"...)
  • Types d'activité : [N] (ex: Email, Appel, Réunion, To-Do + [X custom])
  • Stages : [N] → [liste noms dans l'ordre]
  • Équipes helpdesk : [N]
  • Tags : [N]

[Si sudokeys aussi synchronisé]
Instance sudokeys :
  • ...

⚠️ Nouveautés détectées depuis la dernière sync :
  • [liste des éléments ajoutés/modifiés si comparaison possible]

Prochain rappel de sync recommandé : dans 30 jours
```

6. **Si des nouveaux templates ou types d'activité sont détectés** par rapport au cache précédent :
   Afficher une alerte listant les nouveaux éléments et leur impact potentiel sur le plugin.
