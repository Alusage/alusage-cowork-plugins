---
description: "Démarrer, arrêter ou redémarrer une instance Docker pour une branche de dev alusage via le MCP server Jarvis."
allowed-tools: Bash, mcp__alusage-jarvis__docker_branch, mcp__alusage-jarvis__deployment, mcp__alusage-jarvis__client
argument-hint: "[client] [start|stop|restart|status] [branche optionnelle]"
---

# Commande : /deploy-branch

Tu gères une instance Docker pour une branche de développement alusage via le MCP server Jarvis.

## Étapes

### 1. Identifier les paramètres
- Extraire depuis `$ARGUMENTS` : `{client}`, `{action}` (start/stop/restart/status), `{branche}`
- Si non précisé, détecter la branche active :
  ```bash
  cd /home/njeudy/dev/Devops/odoo_jarvis_assistant/clients/{client}
  git branch --show-current
  ```

### 2. Exécuter l'action via MCP Jarvis

**Démarrer une instance** :
Utiliser l'outil MCP `docker_branch` avec :
- `action`: "start"
- `client`: {client}
- `branch`: {branche}

**Arrêter une instance** :
- `action`: "stop"

**Redémarrer** :
- `action`: "restart"

**Statut** :
- `action`: "status"
Ou utiliser l'outil MCP `deployment` pour voir tous les déploiements actifs.

### 3. Afficher le résultat
- URL d'accès à l'instance si disponible
- Port ou sous-domaine
- Status des conteneurs

### 4. Fallback bash si MCP indisponible
```bash
cd /home/njeudy/dev/Devops/odoo_jarvis_assistant/clients/{client}
# Démarrer
docker compose -f docker-compose.yml up -d

# Voir les logs
docker compose logs -f --tail=50

# Arrêter
docker compose down
```

## Note sur la production

⚠️ La production Cloudron ne se fait PAS via cette commande.
Pour un déploiement Cloudron (prod), utiliser l'outil MCP `cloudron_tools`.
Seules les branches `{version}` (18.0, 19.0...) peuvent être déployées en prod — jamais `master`.
