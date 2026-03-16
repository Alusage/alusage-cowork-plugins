---
description: Démarrer l'instance Odoo locale d'un client avec brainkeys
allowed-tools: Bash(source:*), Bash(brainkeys:*), Bash(docker:*), Read
argument-hint: "[client] [--avec-dump]"
---

Démarre l'instance Odoo + PostgreSQL locale du client via brainkeys.

## Instructions

1. **Détermine le client** depuis `$ARGUMENTS`
   - Si non précisé → demander lequel parmi les 24 clients disponibles
   - Clients : aca, atemis, bh, ecostab, emph, evolusolar, fpv, gazdom, gdc12, genergie, gpx, grc12, indus, petits, protex, provencelia, scomo, securidom, tecsol, tevah, thermoceram

2. **Vérifier l'état actuel** :
   ```bash
   docker ps --filter "name=odoo-{client}" --format "table {{.Names}}\t{{.Status}}"
   ```
   - Si déjà en cours → informer l'utilisateur et proposer de continuer ou arrêter

3. **Si `--avec-dump`** est précisé, télécharger d'abord le dump :
   ```bash
   source /home/njeudy/venv_3.12/bin/activate
   brainkeys datagraber {client}
   ```

4. **Démarrer l'instance** :
   ```bash
   source /home/njeudy/venv_3.12/bin/activate
   brainkeys start-odoo {client}
   ```

5. **Confirmer le démarrage** :
   - Vérifier que les containers sont up : `docker ps --filter "name={client}"`
   - URL locale typique : `http://{client}.localhost` (via Traefik)
   - Afficher l'URL d'accès

6. **Résumé** :
   ```
   ✅ Instance {client} démarrée

   PostgreSQL : postgresql-{client} → running
   Odoo       : odoo-{client}       → running
   URL        : http://{client}.localhost

   Pour ouvrir un shell Odoo :
   source /home/njeudy/venv_3.12/bin/activate && brainkeys odoo {client}
   ```
