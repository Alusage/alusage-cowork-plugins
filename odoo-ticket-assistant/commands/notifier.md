---
description: Envoyer une notification Slack ou Discord sur un ticket
allowed-tools: Read, Bash(curl:*), Bash(cat:*)
argument-hint: "[message] [canal: slack|discord|tous] [ticket: #REF]"
---

Envoie une notification sur Slack (sudokeys) ou Discord (alusage) avec un résumé d'action sur un ticket.

## Instructions

1. **Charge la config** : lire `${CLAUDE_PLUGIN_ROOT}/config/notifications.json`

2. **Détermine le canal cible** depuis "$ARGUMENTS" :
   - `slack` → envoyer sur Slack uniquement (si enabled)
   - `discord` → envoyer sur Discord uniquement (si enabled)
   - `tous` ou non précisé → envoyer sur les deux si activés

3. **Compose le message** de notification :
   ```
   🎫 Ticket #[REF] — [Titre]
   Client : [Nom client]
   Action : [description de l'action effectuée]
   Confiance : [score]/100
   Statut : [AUTO envoyé / En attente validation / Escaladé]
   ```

4. **Envoi Slack** (si `slack.enabled = true`) :
   Utiliser le connecteur MCP Slack (`mcp__slack__slack_send_message`) :
   - channel : valeur de `slack.channel`
   - message : message composé ci-dessus

5. **Envoi Discord** (si `discord.enabled = true`) :
   Utiliser curl vers le webhook Discord :
   ```bash
   curl -s -X POST "[webhook_url]" \
     -H "Content-Type: application/json" \
     -d '{"content": "[message]", "username": "Odoo Support Bot"}'
   ```
   - Remplacer `[webhook_url]` par la valeur de `discord.webhook_url`
   - Si `discord.mention_role` est défini → préfixer avec `<@&ROLE_ID>`

6. **Confirme les envois** :
   ```
   ✅ Notification envoyée sur [Slack / Discord / les deux]
   ```

## Cas d'usage typiques

Ce command est appelé automatiquement par d'autres commandes :
- Par `/repondre-ticket` après un envoi confirmé
- Par `/traiter-auto` après un batch
- Manuellement pour notifier d'une escalade

## Configuration initiale

Pour activer les notifications :
1. Discord : récupérer l'URL webhook dans les paramètres du canal Discord → Intégrations → Webhooks
2. Mettre `"enabled": true` et renseigner `webhook_url` dans `config/notifications.json`
3. Slack : connecter le MCP Slack dans Cowork, puis mettre `"enabled": true`
