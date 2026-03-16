---
description: "Voir le statut de développement alusage : worktrees actifs, branches, instances Docker en cours, PRs GitHub ouvertes."
allowed-tools: Bash, mcp__alusage-jarvis__deployment, mcp__alusage-jarvis__client, mcp__alusage-jarvis__git
argument-hint: "[client optionnel]"
---

# Commande : /dev-status-alusage

Tu affiches un tableau de bord de l'état du développement alusage.

## Étapes

### 1. Worktrees actifs (tous les clients ou un seul)

```bash
# Tous les clients
for client in /home/njeudy/dev/Devops/odoo_jarvis_assistant/clients/*/; do
  name=$(basename $client)
  wt=$(git -C $client worktree list 2>/dev/null | grep -v "bare\|$(git -C $client branch --show-current 2>/dev/null)" | wc -l)
  if [ "$wt" -gt "0" ]; then
    echo "=== $name ==="
    git -C $client worktree list 2>/dev/null
    echo
  fi
done
```

### 2. Déploiements Docker actifs

Utiliser l'outil MCP `deployment` avec action "list" pour obtenir tous les déploiements actifs.

Fallback bash :
```bash
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}" | grep -i odoo
```

### 3. PRs GitHub ouvertes (par client)

Pour chaque client ayant un repo GitHub configuré :
```bash
python3 -c "
import json, os, subprocess
clients_dir = '/home/njeudy/dev/Devops/odoo_jarvis_assistant/clients'
for client in sorted(os.listdir(clients_dir)):
    cfg_path = f'{clients_dir}/{client}/project_config.json'
    if not os.path.exists(cfg_path):
        continue
    cfg = json.load(open(cfg_path))
    g = cfg.get('git', {}).get('remote', {})
    ns = g.get('namespace', '')
    proj = g.get('project_name', '')
    if ns and proj:
        print(f'{client}: {ns}/{proj}')
" 2>/dev/null | while read line; do
  client=$(echo $line | cut -d: -f1)
  repo=$(echo $line | cut -d' ' -f2)
  echo "=== PRs $client ($repo) ==="
  gh pr list --repo "$repo" --state open 2>/dev/null || echo "(pas d'accès ou aucune PR)"
  echo
done
```

### 4. Résumé formaté

```
=== Status Dev Alusage ===

WORKTREES ACTIFS:
  {client}: {branche} ({chemin})

INSTANCES DOCKER:
  {client}/{branche}: {URL} [{statut}]

PRs OUVERTES:
  {client} ({repo}): #{num} — {titre} [{statut}]

DERNIÈRE SYNC ARCHITECTURE:
  CLAUDE.md: {date}
  Architecture guardian: {date}
```
