# Grille de Qualification Détaillée

## Signaux de catégorisation

### BUG
- Mots clés : "erreur", "bug", "ne fonctionne plus", "problème", "bloqué", "crash", "500", "traceback"
- Pattern : fonctionnalité qui marchait et ne marche plus
- Action typique : reproduire, identifier la cause, patch ou contournement

### QUESTION / FORMATION
- Mots clés : "comment", "comment faire", "est-ce possible", "expliquer", "je ne trouve pas"
- Pattern : l'utilisateur cherche une fonctionnalité existante
- Action typique : réponse avec explication + lien documentation si disponible

### CONFIG
- Mots clés : "paramètre", "configurer", "activer", "désactiver", "réglage"
- Pattern : fonctionnalité existe mais n'est pas activée ou mal configurée
- Action typique : instruction de configuration pas à pas

### FEATURE
- Mots clés : "pouvoir", "ajouter", "nouvelle fonction", "amélioration", "serait bien"
- Pattern : demande de quelque chose qui n'existe pas encore
- Action typique : noter la demande, feedback produit, estimation

### INTEGRATION
- Mots clés : "API", "connexion", "synchronisation", "import", "export", "EDI", "webhook"
- Pattern : problème à l'interface entre Odoo et un système externe
- Action typique : analyser les logs d'intégration, vérifier la configuration

## Priorité vs Urgence

La priorité Odoo (0-3 étoiles) n'est pas toujours fiable car fixée par le client.
Évaluer aussi :
- Nombre d'utilisateurs bloqués
- Impact financier mentionné
- Ancienneté du ticket (ticket > 5 jours sans réponse = urgence implicite)
- Client VIP (vérifier dans `knowledge/clients/`)

## Patterns de résolutions réutilisables

Un ticket est candidat à une réponse automatique (score ≥ 85) si :
1. Une résolution identique ou très proche existe dans `knowledge/resolutions/`
2. La réponse ne nécessite pas d'accès à l'environnement spécifique du client
3. La réponse est un guide pas-à-pas ou une explication qui reste valide

Un ticket nécessite validation (score 60-84) si :
1. La situation ressemble à un cas connu mais avec des variantes
2. La réponse implique une action irréversible (suppression, modification de données)
3. Le client est nouveau et son contexte est peu documenté

## Apprentissage continu

Après chaque ticket résolu, alimenter la base de connaissance :

```
knowledge/resolutions/YYYY-MM-DD-ref-ticket-titre-court.md
```

Format recommandé :
```markdown
# Résolution : [titre court]
Date: YYYY-MM-DD
Ticket: #REF
Client: Nom
Catégorie: [BUG/QUESTION/CONFIG/FEATURE/INTEGRATION]
Confiance utilisée: [score]

## Problème
[Description du problème en 1-3 phrases]

## Cause
[Cause racine identifiée]

## Solution
[Solution appliquée, étape par étape si pertinent]

## Tags
[fonctionnalité concernée, version, module Odoo]
```
