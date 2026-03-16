---
description: Créer une Merge Request GitLab depuis une branche TI
allowed-tools: Bash(git:*), Bash(curl:*), Bash(python3:*), Read, Glob
argument-hint: "[#REF] [client] [--draft]"
---

Crée une Merge Request sur GitLab self-hosted depuis la branche TI{REF} vers master.

## Instructions

1. **Charger la config** : lire `${CLAUDE_PLUGIN_ROOT}/config/workspace.json` pour `gitlab.api_url` et `gitlab.token_env`

2. **Vérifier GITLAB_TOKEN** :
   ```bash
   echo ${GITLAB_TOKEN:0:5}...
   ```
   Si vide → informer l'utilisateur : "Exportez `GITLAB_TOKEN` dans votre shell : `export GITLAB_TOKEN=votre-token`"

3. **Identifier le repo** depuis `$ARGUMENTS` (client ou REF) :
   - Chemin repo : `{workspace_root}/{client}/odoo/addons-store/{client}-addons`
   - Récupérer l'URL remote origin : `git -C {path} remote get-url origin`
   - Extraire le chemin GitLab : `sudokeys/{client}-addons`

4. **Trouver l'ID du projet GitLab** :
   ```bash
   curl -s "https://gitlab.sudokeys.com/api/v4/projects?search={client}-addons" \
     -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
     | python3 -c "import json,sys; projects=json.load(sys.stdin); [print(p['id'], p['path_with_namespace']) for p in projects]"
   ```

5. **Vérifier que la branche TI{REF} existe et est pushée** :
   ```bash
   git -C {path} branch -a | grep TI{REF}
   # Si non pushée :
   git -C {path} push -u origin TI{REF}
   ```

6. **Construire le titre et la description de la MR** :

   **Titre** : `TI{REF}: {titre du ticket Odoo}`

   **Description** (format standard Sudokeys) :
   ```markdown
   ## Ticket Odoo
   #REF — {titre du ticket}
   Client : {client}
   Priorité : {priorité}

   ## Résumé des changements
   {résumé en 2-5 phrases de ce qui a été fait}

   ## Modules impactés
   - `{client}_{module1}` : {changement}
   - `{client}_{module2}` : {changement}

   ## Tests effectués
   - [ ] Upgrade du module en local (brainkeys)
   - [ ] Critère d'acceptation 1
   - [ ] Critère d'acceptation 2

   ## Checklist
   - [ ] Pas de print/debug laissé dans le code
   - [ ] Traductions i18n mises à jour si nouveaux termes
   - [ ] Security rules vérifiées
   - [ ] Pas de breaking change sur les données existantes
   ```

   Lire `docs/stories/TI{REF}*.md` et `docs/architecture/TI{REF}*.md` si disponibles pour enrichir la description.

7. **Créer la MR via l'API GitLab** :
   ```bash
   curl -s -X POST "https://gitlab.sudokeys.com/api/v4/projects/{project_id}/merge_requests" \
     -H "PRIVATE-TOKEN: $GITLAB_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
       "source_branch": "TI{REF}",
       "target_branch": "master",
       "title": "{DRAFT_PREFIX}TI{REF}: {titre}",
       "description": "{description}",
       "remove_source_branch": true
     }'
   ```
   - Si `--draft` dans `$ARGUMENTS` → préfixer le titre avec `Draft: `

8. **Résumé** :
   ```
   ✅ MR créée sur GitLab

   Titre : TI{REF}: {titre}
   Branche : TI{REF} → master
   URL : https://gitlab.sudokeys.com/sudokeys/{client}-addons/-/merge_requests/{iid}
   Statut : {Draft/Ready for review}

   Prochaines étapes :
   → Répondre au ticket Odoo avec /repondre-ticket {REF}
   → Planifier une activité de revue avec /planifier-activite {REF} email J+1
   ```
