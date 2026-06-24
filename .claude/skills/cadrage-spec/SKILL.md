---
name: cadrage-spec
description: |
  Rédige une spécification au format maison WAOUHMONDE dans specs/ à partir d'une
  intention métier. À utiliser quand l'utilisateur veut cadrer une nouvelle feature,
  écrire une spec, ou transformer un besoin en exigences. Pose d'abord les questions
  qui lèvent l'ambiguïté. NE PAS utiliser pour coder ou pour modifier du code existant.
---

# Cadrage de spécification (format WAOUHMONDE)

## Quand l'utiliser
- L'utilisateur décrit un besoin/feature et veut une spec exploitable par Claude Code.

## Quand NE PAS l'utiliser
- Pour écrire ou modifier du code (cette skill produit de la spec, pas du code).
- Pour de la documentation utilisateur (voir `doc-vivante`).

## Démarche
1. **Poser les questions** qui lèvent l'ambiguïté AVANT d'écrire (périmètre, utilisateurs,
   contraintes, cas limites connus, volumétrie, sécurité). Ne rien inventer en silence.
2. Créer `specs/00X-<capacite>.md` avec EXACTEMENT ces sections :
   - **But & contexte** (le « pourquoi » métier, pas seulement la solution)
   - **Périmètre** + **Non-goals** (explicites)
   - **Exigences fonctionnelles** en **Gherkin** (Étant donné / Quand / Alors) :
     nominal + cas d'erreur + cas limites
   - **Contrats d'API** (endpoints, entrées/sorties, codes d'erreur)
   - **Modèle de données / schémas**
   - **Dépendances** avec **versions épinglées**
   - **Critères d'acceptation vérifiables** (commande de test ou métrique mesurable)
3. Proposer une **décomposition en lots** indépendants + l'**ordre d'implémentation**.
4. Signaler les **décisions structurantes** à tracer en ADR (`specs/adr/`).

## Format
Markdown pour la narration, YAML plat pour la config structurée. Éviter JSON/XML profonds.

## Anti-patterns
- Pas de chemin heureux uniquement : toujours erreurs + limites.
- Pas de critère d'acceptation non mesurable.
- Ne pas coder : cette skill produit de la spec, pas du code.
- Ne pas laisser d'ambiguïté implicite « pour plus tard ».
