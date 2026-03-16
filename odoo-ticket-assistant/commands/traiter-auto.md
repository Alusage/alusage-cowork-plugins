---
description: Traitement automatique de tous les tickets à haute confiance
allowed-tools: mcp__odoo-alusage__search_records, mcp__odoo-alusage__get_record, mcp__odoo-alusage__create_record, Read, Glob, Write
argument-hint: "[seuil: 85-100] [instance: sudokeys] [dry-run]"
---

Traite en batch tous les tickets dont le score de confiance dépasse le seuil (défaut : 85/100).
En mode `dry-run`, génère les réponses sans les envoyer pour validation.

## Instructions

1. **Paramètres depuis "$ARGUMENTS"** :
   - Seuil de confiance : valeur numérique si présente, sinon 85
   - Instance : sudokeys si mentionné, sinon principale
   - Mode : `dry-run` si mentionné → ne pas envoyer, juste générer

2. **Récupère les 30 tickets ouverts les plus récents** (même logique que `/analyser-tickets`)

3. **Pour chaque ticket, en séquence** :
   a. Calculer le score de confiance
   b. Si score ≥ seuil → générer la réponse
   c. Collecter toutes les réponses générées

4. **Présente un récapitulatif avant envoi** :
   ```
   # Traitement automatique — [N] tickets sélectionnés

   | # | Ticket | Client | Score | Catégorie | Réponse (aperçu) |
   |---|--------|--------|-------|-----------|-----------------|
   | 1 | #3332  | Nom    | 92    | CONFIG    | "Voici la proc..." |
   | 2 | #3067  | Nom    | 88    | QUESTION  | "Pour accéder à..." |

   ⚠️ Ces [N] réponses vont être envoyées. Confirmer ? (oui/non)
   ```

5. **Si mode dry-run** : Afficher toutes les réponses sans demander confirmation d'envoi

6. **Si confirmé** : Envoyer chaque réponse via Odoo et reporter les succès/échecs

7. **Rapport final** :
   ```
   ✅ Traitement terminé
   Envoyés : [N]/[N]
   Échecs : [N] (si applicable)

   Souhaitez-vous sauvegarder ces résolutions dans la base de connaissance ?
   ```
