---
description: "Démarrer une session de développement Alusage : créer une branche dev-YYYYMMDD-HHMM, un worktree, et optionnellement lancer l'instance de test."
allowed-tools: Bash, Read, mcp__alusage-jarvis__worktree, mcp__alusage-jarvis__branch, mcp__alusage-jarvis__docker_branch, mcp__alusage-jarvis__client, mcp__alusage-jarvis__git
argument-hint: "[client] [description optionnelle]"
---

# Commande : /dev-alusage

Tu es en train de démarrer une session de développement Alusage.

## Étapes

### 1. Lire les règles d'architecture
```bash
head -60 /home/njeudy/dev/Devops/odoo_jarvis_assistant/CLAUDE.md
```
Retenir les 5 règles du gardien d'architecture.

### 2. Identifier le client
- Si `$ARGUMENTS` contient un nom de client → l'utiliser
- Sinon, lister les clients disponibles :
  ```bash
  ls /home/njeudy/dev/Devops/odoo_jarvis_assistant/clients/
  ```
  Demander à l'utilisateur quel client.

### 3. Lire la config du client
```bash
cat /home/njeudy/dev/Devops/odoo_jarvis_assistant/clients/{client}/project_config.json
```
Noter la version Odoo, le repo GitHub, les branches de production autorisées.

### 4. Générer le nom de branche
```bash
BRANCH=$(date "+dev-%Y%m%d-%H%M")
echo "Branche : $BRANCH"
```

### 5. Créer le worktree via MCP Jarvis
Utiliser l'outil MCP `worktree` avec les paramètres :
- `action`: "create"
- `client`: {client}
- `branch`: {BRANCH}

Si le MCP n'est pas disponible, fallback bash :
```bash
cd /home/njeudy/dev/Devops/odoo_jarvis_assistant/clients/{client}
git checkout -b {BRANCH}
git worktree add .worktrees/{BRANCH} {BRANCH}
echo "✅ Worktree créé : .worktrees/{BRANCH}"
```

### 6. (Optionnel) Démarrer l'instance de test
Si l'utilisateur veut tester, utiliser l'outil MCP `docker_branch` :
- `action`: "start"
- `client`: {client}
- `branch`: {BRANCH}

Sinon confirmer que le worktree est prêt et donner le chemin :
`/home/njeudy/dev/Devops/odoo_jarvis_assistant/clients/{client}/.worktrees/{BRANCH}/`

### 7. Résumé
Afficher :
```
✅ Session de dev démarrée
Client    : {client} (Odoo {version})
Branche   : {BRANCH}
Worktree  : clients/{client}/.worktrees/{BRANCH}/
GitHub    : Alusage/odoo_alusage → branche {version_prod}
Instance  : {URL si démarrée, sinon "non démarrée"}
```

### 8. BMAD (optionnel)
Si l'utilisateur a une tâche précise, proposer de lancer BMAD :
- "Veux-tu que je crée une User Story pour cette tâche ? (/dev-alusage avec BMAD)"
