---
description: "Créer une Pull Request GitHub pour la branche de dev alusage en cours. Le repo cible est lu depuis project_config.json du client."
allowed-tools: Bash, Read, mcp__alusage-jarvis__git, mcp__alusage-jarvis__client
argument-hint: "[client] [branche optionnelle]"
---

# Commande : /create-pr-alusage

Tu vas créer une Pull Request GitHub pour une branche de développement alusage.

## Étapes

### 1. Identifier le client et la branche
- Si `$ARGUMENTS` contient un client → utiliser
- Sinon demander
- Détecter la branche active :
  ```bash
  cd /home/njeudy/dev/Devops/odoo_jarvis_assistant/clients/{client}
  git branch --show-current
  ```

### 2. Lire le repo GitHub depuis la config client
```bash
python3 -c "
import json
config = json.load(open('/home/njeudy/dev/Devops/odoo_jarvis_assistant/clients/{client}/project_config.json'))
g = config.get('git', {}).get('remote', {})
print('repo:', g.get('url', ''))
print('org:', g.get('namespace', ''))
print('project:', g.get('project_name', ''))
pub = config.get('publication', {})
print('branches_prod:', pub.get('allowed_branches', []))
print('version:', config.get('odoo_version', ''))
"
```

- **Repo** = `{org}/{project_name}` (ex: `Alusage/odoo_alusage`, ou autre selon le client)
- **Branche cible** = la version Odoo du client (ex: `18.0`) — pas `master`

### 3. Récupérer les commits de la branche
```bash
cd /home/njeudy/dev/Devops/odoo_jarvis_assistant/clients/{client}
git log {version_branche}..{branche_dev} --oneline
```

### 4. Construire le titre et le corps de la PR
- **Titre** : extraire la date de la branche `dev-YYYYMMDD-HHMM` → "Dev YYYYMMDD HHMM"
  ```bash
  echo "{BRANCH}" | sed 's/dev-\([0-9]*\)-\([0-9]*\)/Dev \1 \2/'
  ```
- **Corps** : résumé des commits, modules impactés, description des changements

### 5. Vérifier que la branche est poussée
```bash
cd /home/njeudy/dev/Devops/odoo_jarvis_assistant/clients/{client}
git push origin {BRANCH} --set-upstream 2>&1
```

### 6. Créer la PR avec `gh`
```bash
gh pr create \
  --repo {org}/{project_name} \
  --title "Dev {YYYYMMDD} {HHMM}" \
  --base {version_branche} \
  --head {BRANCH} \
  --body "$(cat <<'EOF'
## Résumé
{description des changements}

## Modules impactés
{liste des modules modifiés}

## Tests effectués
- [ ] Instance de test démarrée et vérifiée
- [ ] Critères d'acceptation validés
- [ ] Pas de régression sur les autres modules

## Notes
{notes complémentaires si besoin}
EOF
)"
```

### 7. Confirmer
Afficher l'URL de la PR créée et proposer de répondre sur le ticket Odoo si concerné.
