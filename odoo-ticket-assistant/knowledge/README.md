# Base de Connaissance — odoo-ticket-assistant

Cette base de connaissance est le moteur d'amélioration continue du plugin.
Plus elle est riche, plus les réponses automatiques sont fiables.

## Structure

```
knowledge/
├── projets/        # Documentation par projet client
├── versions/       # Changelogs et notes de version
├── resolutions/    # Résolutions de tickets passés (apprentissage)
└── clients/        # Fiches clients (contexte, modules, contrat)
```

## Comment alimenter la base

### Via Claude (recommandé)
Utilise la commande `/documenter-projet` :
- `/documenter-projet client NomClient` — ajouter/mettre à jour une fiche client
- `/documenter-projet projet NomProjet` — documenter un projet
- `/documenter-projet version 17.0.2` — ajouter un changelog
- Après chaque ticket résolu, Claude proposera automatiquement de sauvegarder la résolution

### Manuellement
Créer des fichiers `.md` directement dans les sous-dossiers en respectant les formats documentés.

## Impact sur le score de confiance

| Source de connaissance | Impact sur score |
|-----------------------|-----------------|
| Résolution identique dans `resolutions/` | +40 pts |
| Résolution similaire dans `resolutions/` | +20-35 pts |
| Documentation projet dans `projets/` | +10-30 pts |
| Fiche client dans `clients/` | +5-10 pts |
| Changelog version dans `versions/` | +5-20 pts |

## Objectif : atteindre 80%+ de tickets en AUTO

Jalons :
- 0 résolutions → ~10% tickets en AUTO (questions génériques seulement)
- 20 résolutions → ~30% tickets en AUTO
- 50 résolutions → ~50% tickets en AUTO
- 100 résolutions → ~70%+ tickets en AUTO
- Base clients complète → +10% supplémentaires

## Maintenance

- Archiver les résolutions de plus de 2 ans ou pour des versions Odoo obsolètes
- Mettre à jour les fiches clients à chaque changement de version ou de modules
- Revoir les résolutions marquées "à vérifier" régulièrement
