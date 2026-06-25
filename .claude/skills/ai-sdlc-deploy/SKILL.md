---
name: ai-sdlc-deploy
description: Phase 5 du AI-SDLC — Déploiement. Revue (Logic Review), hooks de sécurité, portes CI/CD, release progressive. À utiliser avant et pendant une mise en production.
---

# Phase 5 — Déploiement (exécution Claude Code)
**Entrées :** code testé/évalué (phase 4).

## Étapes
1. Revue de premier passage : `/code-review` (ou `/review` sur une PR), ou subagent « sécurité » + vérification adversariale. Vérifie bugs, secrets, imports hallucinés, tests affaiblis.
2. Logic Review : résume le diff/PR en clair, mappe chaque changement à un critère de `@specs/`, liste les risques résiduels, présente à l'humain. Pas d'« Approve » réflexe.
3. Vérifie les hooks (`.claude/settings.json`) : PreToolUse (bloque secrets/écritures prod/commandes destructives), PostToolUse (formate/linte).
4. Portes CI/CD bloquantes : SAST + SCA, scan de secrets, SBOM/signatures, tests + evals au seuil. Headless : `claude -p "<revue>" --output-format json`.
5. Sécurité d'exécution : sandbox isolée/éphémère, dépendances de registres vérifiés versions épinglées, identité dédiée droits au plus juste jetons courts, jamais les droits admin du dev, pas de données de prod par défaut.
6. Release progressive : canary / feature flags + surveillance + auto-rollback. Checkpoint avant toute modif d'infra. Sépare prototype et production.

## Porte de sortie
- [ ] Revue IA + revue humaine finale (chaque ligne relue).
- [ ] Hooks de sécurité actifs.
- [ ] CI/CD (SAST, SCA, secrets, SBOM) bloquants au vert.
- [ ] Exécution avec identité dédiée, jamais en prod par défaut.
- [ ] Release progressive + auto-rollback + checkpoints.

## STOP — approbation humaine explicite requise
Déploiement prod, changement de schéma BDD, transaction financière, suppression de données, modification d'infrastructure.

## Interdits
- Approuver/fusionner sans Logic Review.
- Mettre les règles de sécurité uniquement dans le prompt (utiliser des hooks).
- Donner à l'agent les droits admin du développeur.
- Déployer sans plan de rollback.

## Skills utiles (skill-finder)
Avant de coder a la main, verifie s'il existe une skill adaptee : invoque `skill-finder` (mots-cles : CI/CD, vercel, docker, release, changelog). Installe seulement apres accord humain.
