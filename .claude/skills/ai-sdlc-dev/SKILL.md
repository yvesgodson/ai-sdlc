---
name: ai-sdlc-dev
description: Phase 3 du AI-SDLC — Développement. Boucle par lot explore → plan → TDD → implémente → vérifie. À utiliser pour implémenter une feature à partir d'une spec validée.
---

# Phase 3 — Développement (exécution Claude Code)
**Entrées :** `specs/` + contrats figés + ADR ; bundle handoff UI si applicable.

## Boucle par lot
1. EXPLORER : lis `@specs/00X-...` et les fichiers concernés, ne modifie rien (subagent si volumineux).
2. PLANIFIER (mode plan) : plan fichier par fichier mappé aux critères ; attends l'OK humain.
3. TESTER D'ABORD : écris les tests dérivés des scénarios Gherkin (pas de test trivial).
4. IMPLÉMENTER : code jusqu'au vert, calque le style voisin, montre le diff après chaque fichier.
5. BOUCLER (bornée) : lance les tests, corrige la cause racine, relance. Arrêt = tout vert OU 5 itérations. Ne modifie jamais les tests pour les faire passer.
6. VÉRIFIER : résume le diff, mappe chaque changement à un critère, vérifie que chaque import correspond à un paquet réel.
7. COMMITTER : message Conventional Commits lié à la spec. Pas de commit sans accord.

## Parallélisation
Lots indépendants : 1 subagent par lot, en worktrees isolés si risque de conflit. Modèle adapté (Opus/Sonnet/Haiku).

## MCP
Consomme un serveur MCP existant plutôt qu'un wrapper. Audite-le avant accès ; credentials via variables d'environnement ; pas de données de prod ; lecture seule par défaut ; montre les entrées avant un appel sensible.

## Porte de sortie
- [ ] Code de chaque lot conforme aux contrats.
- [ ] Tests dérivés des scénarios, au vert.
- [ ] Diffs relus par un humain.
- [ ] Imports vérifiés (aucun paquet halluciné).
- [ ] Commits propres liés aux specs.

## Interdits
- Faire confiance aux 80 % « qui ont l'air bons » sans vérifier limites/erreurs.
- Gros diff inauditable.
- Modifier les tests pour les faire passer.
- Empiler des correctifs sur une mauvaise base → `/rewind` et reformuler.

## Skills utiles (skill-finder)
Avant de coder a la main, verifie s'il existe une skill adaptee : invoque `skill-finder` (mots-cles : testing, TDD, code-review, refactor, debugging). Installe seulement apres accord humain.
