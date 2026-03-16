---
description: Vue d'ensemble des branches TI actives sur un ou tous les clients
allowed-tools: Bash(git:*), Bash(ls:*), Bash(source:*), Bash(curl:*), Bash(python3:*), Read
argument-hint: "[client] [--all]"
---

Affiche les branches TI actives dans les repos clients, avec leur âge et état (locale/pushée/MR ouverte).

## Instructions

1. **Charger la config** workspace

2. **Déterminer le périmètre** :
   - Nom de client précisé → analyser uniquement ce client
   - `--all` → analyser tous les clients
   - Sinon → analyser les 5 derniers clients modifiés

3. **Pour chaque client dans le périmètre** :

   a. Trouver le repo : `{workspace_root}/{client}/odoo/addons-store/{client}-addons`
   b. Lister les branches TI actives :
   ```bash
   git -C {repo} branch | grep "TI" | sed 's/[* ]*//'
   ```
   c. Pour chaque branche TI{N} :
   - Date du dernier commit : `git -C {repo} log TI{N} -1 --format="%ar"`
   - Nombre de commits depuis master : `git -C {repo} rev-list master..TI{N} --count`
   - Statut push : `git -C {repo} branch -vv | grep TI{N}`

4. **Afficher le rapport** :

```
# Statut branches dev — {DATE}

## {client_1}
  TI{N1} — il y a 2 jours — 3 commits — ✅ pushée
  TI{N2} — il y a 5 heures — 1 commit — ⚠️ non pushée
  TI{N3} — il y a 3 semaines — 8 commits — ✅ pushée

## {client_2}
  TI{N4} — il y a 1 jour — 2 commits — ✅ pushée
  (aucune branche TI active)

---
Total : {N} branches actives sur {M} clients
Branches anciennes (>2 semaines) : {liste}
```

5. **Détecter les anomalies** et les signaler :
   - Branche TI non pushée depuis > 1 jour → ⚠️
   - Branche TI existant depuis > 3 semaines sans MR → ⚠️
   - Plusieurs branches TI sur le même client → ℹ️
