---
name: ai-sdlc-test
description: Phase 4 du AI-SDLC — Test & QA. Tests déterministes + evals (LLM-as-judge), 7 dimensions de qualité, revue par subagents. À utiliser pour valider la qualité avant déploiement.
---

# Phase 4 — Test & QA (exécution Claude Code)
**Entrées :** code + tests (phase 3), specs (rubrique de référence).

## Étapes
1. Complète les tests déterministes depuis `@specs/` : nominal + erreurs + cas limites. Chaque test doit pouvoir échouer pour une vraie raison.
2. Tests visuels/comportementaux (si UI) : Playwright — états vide/chargement/erreur, responsive, comparaison contre la maquette.
3. LLM-as-judge : second appel qui note la sortie contre une rubrique de la spec (0–5 + justification) sur intention, conventions, gestion d'erreurs. Headless : `claude -p "<prompt d'éval>" --output-format json`.
4. Sécurité : SAST/SCA (code et dépendances vulnérables ou hallucinées), scan de secrets, red-team scriptée pour les refus.
5. Inspection de trajectoire : bons fichiers lus d'abord ? séquence d'édition sensée ? bon outil/skill appelé ?

## 7 dimensions à contrôler
1. Intention (humain + LLM-judge). 2. Correction fonctionnelle (tests). 3. Correction visuelle/comportementale (Playwright). 4. Coût & efficacité (tokens, latence, nb d'itérations). 5. Qualité & conventions. 6. Trajectoire. 7. Auto-réparation.

## Revue exhaustive (subagents)
1 subagent par dimension (bugs, sécurité, perf, conventions) sur le diff → 1 subagent « sceptique » tente de réfuter chaque trouvaille → ne garder que celles qui survivent → rapport trié par gravité.

## Porte de sortie
- [ ] Tests déterministes au vert (nominal + erreurs + limites).
- [ ] Tests visuels (si UI).
- [ ] LLM-as-judge : scores au-dessus du seuil.
- [ ] SAST/SCA/secrets passés (aucune dépendance hallucinée).
- [ ] Inspection de trajectoire faite.
- [ ] Rapport de revue sans alerte critique.

## Interdits
- Confondre « ça compile » avec « c'est conforme ».
- Accepter des tests supprimés/mockés pour « passer ».
- Juger le code sans juger le rendu (UI).
- Garder des fausses alertes non vérifiées.

## Skills utiles (skill-finder)
Avant de coder a la main, verifie s'il existe une skill adaptee : invoque `skill-finder` (mots-cles : evals, LLM-as-judge, playwright, accessibility, security). Installe seulement apres accord humain.
