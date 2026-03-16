# Configuration Multi-Instance Odoo

## Instances disponibles

| Alias | Connecteur MCP | Description |
|-------|---------------|-------------|
| Instance principale | `mcp__odoo-alusage__*` | Instance principale connectée |
| sudokeys | `mcp__odoo-sudokeys__*` | Instance sudokeys (à connecter) |

## Comment cibler une instance

Dans les commandes et questions, préciser l'instance si nécessaire :
- "analyse les tickets **sudokeys**" → utiliser `mcp__odoo-sudokeys__*`
- Sans précision → utiliser l'instance principale (`mcp__odoo-alusage__*`)

## Ajouter l'instance sudokeys

Pour connecter l'instance sudokeys, l'utilisateur doit :
1. Ouvrir les paramètres de Cowork
2. Ajouter un connecteur Odoo pour sudokeys
3. Le nom du connecteur MCP sera alors `odoo-sudokeys`

## Modèles Odoo utilisés

| Modèle | Usage |
|--------|-------|
| `helpdesk.ticket` | Tickets support |
| `helpdesk.stage` | Étapes/statuts des tickets |
| `helpdesk.team` | Équipes support |
| `res.partner` | Clients |
| `mail.message` | Messages/commentaires sur tickets |
| `helpdesk.ticket.tag` | Tags des tickets |
