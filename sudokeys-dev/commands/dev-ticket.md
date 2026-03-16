---
description: Lancer le workflow dev complet depuis un ticket Odoo
allowed-tools: mcp__odoo-alusage__get_record, mcp__odoo-alusage__search_records, Read, Write, Glob, Bash(git:*), Bash(ls:*), Bash(source:*), Bash(brainkeys:*), Bash(curl:*), Bash(python3:*), Bash(npx:*), Bash(mkdir:*)
argument-hint: "[#REF] [phase: pm|arch|dev|qa|full] [client: nom]"
---

Lance le workflow BMAD complet (ou une phase spécifique) pour transformer un ticket Odoo en développement structuré avec branche TI, artefacts docs/ et MR GitLab.

## Instructions

1. **Charger la config** : lire `${CLAUDE_PLUGIN_ROOT}/config/workspace.json`

2. **Récupérer le ticket Odoo** depuis `mcp__odoo-alusage__get_record` (helpdesk.ticket) :
   - Champs : `name`, `description`, `partner_name`, `partner_company_name`, `priority`, `tag_ids`, `message_ids`

3. **Identifier le client, le repo et la version Odoo** :
   - Mapper `partner_company_name` → nom de dossier dans `workspace_root`
   - Chemin du repo : `{workspace_root}/{client}/odoo/addons-store/{client}-addons`
   - Si ambiguïté → lister les dossiers et demander confirmation
   - Si `$ARGUMENTS` contient un nom de client → utiliser directement
   - **Détecter la version Odoo** du client :
     ```bash
     grep -r '"version"' {repo_path} --include="__manifest__.py" -m 1 | grep -oP '"\d+\.\d+' | head -1
     # Fallback: grep README
     grep -i "odoo" {workspace_root}/{client}/README.md 2>/dev/null | grep -oP '\d{2}\.\d' | head -1
     ```
   - Stocker dans `{odoo_version}` (ex: `16.0`) pour les phases Architect et Dev
   - Chemins sources Odoo : `/home/njeudy/dev/doc_technique/Odoo/`
     - Core : `odoo/odoo-{odoo_version}/addons/`
     - Enterprise : `enterprise/enterprise-{odoo_version}/`
     - OCA : `oca/{repo}/{repo}-{odoo_version}/`

4. **Déterminer la phase** depuis `$ARGUMENTS` :
   - `full` (défaut) → exécuter toutes les phases séquentiellement
   - `pm` → seulement la User Story
   - `arch` → seulement la Spec technique (doit avoir une story existante)
   - `dev` → seulement l'implémentation (doit avoir une spec existante)
   - `qa` → seulement la validation

5. **Vérifier/créer la branche TI{REF}** :
   ```bash
   cd {repo_path}
   git status
   git branch | grep TI{REF}
   # Si n'existe pas :
   git checkout master && git pull
   git checkout -b TI{REF}
   ```

6. **Phase PM — Générer la User Story** :
   - Appliquer l'Agent PM du skill `bmad-odoo`
   - Chemin : `{repo_path}/docs/stories/TI{REF}-{titre-slug}.md`
   - Créer le dossier `docs/stories/` si absent
   - Présenter la story pour validation avant de continuer

7. **Phase Architect — Générer la Spec technique** :
   - Identifier les modules Odoo concernés (helpdesk, sale, project, etc.)
   - **Consulter les sources Odoo** dans cet ordre :
     a. Lire le modèle natif Odoo : `/home/njeudy/dev/doc_technique/Odoo/enterprise/enterprise-{odoo_version}/{module}/models/` ou `odoo/odoo-{odoo_version}/addons/{module}/models/`
     b. Chercher des patterns OCA : `grep -r "{champ_ou_feature}" /home/njeudy/dev/doc_technique/Odoo/oca/{repo_pertinent}/{repo}-{odoo_version}/ --include="*.py" -l`
     c. Lire le code existant du module client : `Read` sur `{repo_path}/{client}_{module}/models/`
   - Appliquer l'Agent Architect du skill `bmad-odoo`
   - Chemin : `{repo_path}/docs/architecture/TI{REF}-spec.md`
   - Présenter la spec pour validation avant de continuer

8. **Phase Dev — Implémenter** :
   - Lire la spec technique
   - Lire le code existant des modules concernés
   - Implémenter les changements avec Edit/Write
   - Committer chaque sous-tâche : `[TYPE] {module}: {description}`
   - Vérifier la syntaxe Python basique après chaque fichier

9. **Phase QA — Validation** :
   - Lire la User Story et la Spec
   - Lire le code implémenté
   - Générer la checklist QA dans `{repo_path}/docs/stories/TI{REF}-qa.md`
   - Signaler explicitement tout risque de régression

10. **Résumé final** :
    ```
    ✅ Workflow TI{REF} terminé

    Client : {client}
    Repo : {repo_path}
    Branche : TI{REF}
    Fichiers modifiés : [liste]
    Commits : [liste]

    Artefacts créés :
    • docs/stories/TI{REF}-{titre}.md
    • docs/architecture/TI{REF}-spec.md
    • docs/stories/TI{REF}-qa.md

    Prochaines étapes :
    → /start-instance {client} — tester en local
    → /create-mr {REF} — créer la MR GitLab
    → Répondre au ticket Odoo avec /repondre-ticket {REF}
    ```
