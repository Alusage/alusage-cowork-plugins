---
description: Arrêter l'instance Odoo locale d'un client
allowed-tools: Bash(source:*), Bash(brainkeys:*), Bash(docker:*)
argument-hint: "[client]"
---

Arrête l'instance Odoo + PostgreSQL locale du client via brainkeys.

## Instructions

1. **Détermine le client** depuis `$ARGUMENTS`

2. **Vérifier que l'instance tourne** :
   ```bash
   docker ps --filter "name={client}" --format "{{.Names}}"
   ```

3. **Arrêter** :
   ```bash
   source /home/njeudy/venv_3.12/bin/activate
   brainkeys stop-odoo {client}
   ```

4. **Confirmer** :
   ```
   ✅ Instance {client} arrêtée
   ```
