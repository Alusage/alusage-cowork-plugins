---
description: Ajouter de la documentation à la base de connaissance
allowed-tools: Read, Write, Glob, Bash(ls:*)
argument-hint: "[type: projet|version|client|resolution] [nom]"
---

Ajoute ou met à jour un élément dans la base de connaissance du plugin pour améliorer les futures réponses.

## Instructions

1. **Détermine le type de documentation** depuis "$ARGUMENTS" :
   - `projet` → `${CLAUDE_PLUGIN_ROOT}/knowledge/projets/`
   - `version` → `${CLAUDE_PLUGIN_ROOT}/knowledge/versions/`
   - `client` → `${CLAUDE_PLUGIN_ROOT}/knowledge/clients/`
   - `resolution` → `${CLAUDE_PLUGIN_ROOT}/knowledge/resolutions/`
   - Si non précisé, demander à l'utilisateur de préciser le type

2. **Liste les fichiers existants** dans le répertoire cible via Bash `ls`

3. **Collecte le contenu** :
   - Si l'utilisateur a fourni du texte dans sa demande → l'utiliser directement
   - Si l'utilisateur a mentionné un fichier → lire ce fichier avec Read
   - Si rien n'est fourni → demander le contenu à l'utilisateur

4. **Crée ou met à jour le fichier** dans le bon répertoire :

   **Pour un projet** (`projets/nom-projet.md`) :
   ```markdown
   # Projet : [Nom]
   Dernière mise à jour: [DATE]

   ## Description
   [Description fonctionnelle]

   ## Modules Odoo concernés
   [liste des modules]

   ## Personnalisations et développements spécifiques
   [développements custom]

   ## Points d'attention support
   [comportements spécifiques, limites connues]

   ## Contacts projet
   [référent technique, référent client]
   ```

   **Pour une version** (`versions/v[X.Y.Z]-[date].md`) :
   ```markdown
   # Version [X.Y.Z] — [DATE]

   ## Nouveautés
   [liste des nouvelles fonctionnalités]

   ## Corrections
   [bugs corrigés]

   ## Changements comportement
   [comportements modifiés qui peuvent générer des tickets]

   ## Migration
   [points de migration importants]
   ```

   **Pour un client** (`clients/nom-client.md`) :
   ```markdown
   # Client : [Nom]
   Depuis: [date début relation]
   Contrat: [type de contrat support]

   ## Contexte
   [description de leur utilisation d'Odoo]

   ## Modules installés
   [liste]

   ## Version Odoo
   [version]

   ## Personnalisations
   [développements spécifiques]

   ## Historique notable
   [incidents marquants, préférences de communication]

   ## Contacts
   [nom, email, téléphone des interlocuteurs]
   ```

   **Pour une résolution** (`resolutions/[DATE]-[REF]-[titre].md`) :
   Utiliser le format défini dans `skills/ticket-triage/references/grille-qualification.md`

5. **Confirme la sauvegarde** :
   ```
   ✅ Documentation ajoutée : knowledge/[type]/[nom-fichier].md
   Cette information sera utilisée pour scorer les prochains tickets similaires.
   Impact estimé : +[X] points de confiance pour les tickets de type [catégorie]
   ```
