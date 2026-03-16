# Odoo Ticket Assistant

Plugin d'assistance intelligente aux tickets Odoo Helpdesk avec triage automatique, qualification et réponse calibrée selon un niveau de confiance.

## Vue d'ensemble

Ce plugin connecte Claude directement à Odoo Helpdesk pour :
1. **Trier et qualifier** les tickets ouverts automatiquement
2. **Générer des réponses** personnalisées selon le contexte client et projet
3. **Envoyer automatiquement** les réponses à haute confiance (≥ 85/100)
4. **Demander une validation** pour les réponses moins certaines
5. **S'améliorer en continu** grâce à une base de connaissance évolutive

## Commandes disponibles

| Commande | Description | Exemple |
|----------|-------------|---------|
| `/analyser-tickets` | Trier tous les tickets ouverts | `/analyser-tickets urgent` |
| `/repondre-ticket` | Répondre à un ticket spécifique | `/repondre-ticket 3332` |
| `/traiter-auto` | Batch auto sur tickets haute confiance | `/traiter-auto dry-run` |
| `/documenter-projet` | Alimenter la base de connaissance | `/documenter-projet client NomClient` |

## Skills inclus

- **ticket-triage** : qualification, scoring de confiance, catégorisation
- **ticket-response** : rédaction de réponses professionnelles par catégorie

## Connecteurs requis

| Connecteur | Requis | Description |
|-----------|--------|-------------|
| `odoo-alusage` | ✅ Oui | Instance Odoo principale |
| `odoo-sudokeys` | ⚡ Optionnel | Instance Odoo sudokeys |

## Niveaux de confiance

| Score | Label | Action |
|-------|-------|--------|
| 85-100 | 🟢 AUTO | Envoi automatique sans validation |
| 60-84 | 🟡 VALIDATION | Brouillon + validation humaine |
| 30-59 | 🟠 PARTIEL | Réponse partielle + escalade |
| 0-29 | 🔴 ESCALADE | Traitement manuel requis |

## Base de connaissance

Stockée dans `knowledge/`, elle s'enrichit à chaque ticket résolu.
Voir `knowledge/README.md` pour les objectifs et le fonctionnement.

## Support multi-instance

Ajouter "sudokeys" dans n'importe quelle commande pour cibler l'instance sudokeys :
- `/analyser-tickets sudokeys`
- `/repondre-ticket 1234 sudokeys`

## Ce qui améliore le plugin au fil du temps

1. **Sauvegarder les résolutions** après chaque ticket traité → Claude le propose automatiquement
2. **Documenter les projets** dès qu'une nouvelle configuration est mise en place
3. **Maintenir les fiches clients** à jour (version Odoo, modules, spécificités)
4. **Alimenter les changelogs** à chaque déploiement de version

## Lancer Claude Code pour du développement

Si un ticket nécessite un développement Odoo :
- Claude peut ouvrir un terminal et lancer Claude Code dans le dossier du module concerné
- Utiliser `/repondre-ticket [REF]` → Claude détectera si un développement est requis et proposera d'ouvrir Claude Code

## Roadmap d'amélioration suggérée

- [ ] Intégration GitHub/GitLab pour référencer les commits liés aux tickets
- [ ] Webhook Odoo pour traitement en temps réel des nouveaux tickets
- [ ] Dashboard hebdomadaire automatique (ticket stats, temps de réponse)
- [ ] Détection de doublons entre tickets
