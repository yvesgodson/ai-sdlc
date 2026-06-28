---
name: ai-sdlc-test
description: Phase 4 du AI-SDLC — Test & QA. Tests déterministes + evals (LLM-as-judge), 7 dimensions de qualité, revue par subagents. À utiliser pour valider la qualité avant déploiement.
---

# Phase 4 — Test & QA (exécution Claude Code)
**Entrées :** code + tests (phase 3), specs (rubrique de référence).

## Étapes
1. Complète les tests déterministes depuis `@specs/` : nominal + erreurs + cas limites. Chaque test doit pouvoir échouer pour une vraie raison.
2. Tests visuels/comportementaux (si UI) : Playwright — états vide/chargement/erreur, responsive, comparaison contre la maquette.
   - Audit UI/accessibilité : lance la skill `web-design-guidelines` sur les fichiers d'écran rendus (revue contre les Web Interface Guidelines). Outil de REVUE, pas de génération ; traiter ses trouvailles `file:line` comme des écarts à corriger. Écarts récurrents à viser : élément texte masqué en responsive devenant **icône seule** → `aria-label` ; **boutons personnalisés** (hors composant Button) sans `focus-visible:ring` ; résultat asynchrone non annoncé → `aria-live="polite"` ; page sans `h1` (hiérarchie) ; absence de lien d'évitement (skip link).
3. LLM-as-judge : second appel qui note la sortie contre une rubrique de la spec (0–5 + justification) sur intention, conventions, gestion d'erreurs. Headless : `claude -p "<prompt d'éval>" --output-format json`.
4. Sécurité : SAST/SCA (code et dépendances vulnérables ou hallucinées), scan de secrets, red-team scriptée pour les refus.
   - **Triage SCA dev vs prod** : `npm audit` mélange l'outillage (vitest/esbuild, postcss de build) et le runtime. Vérifie l'exposition réelle avec `npm audit --omit=dev` ; une faille « critique » du serveur de dev n'affecte pas la prod. **Ne lance JAMAIS `npm audit fix --force` à l'aveugle** : il peut rétrograder le framework (ex. Next → 9.x). Documente et accepte les vulnérabilités dev/build non corrigeables sans casse.
5. Inspection de trajectoire : bons fichiers lus d'abord ? séquence d'édition sensée ? bon outil/skill appelé ?

## e2e robustes (services externes : IA, paiement)
- **Timeouts généreux** : un appel IA réel + la compilation à froid du serveur dev dépassent les 5 s par défaut. Fixe des timeouts explicites (redirection après inscription ~20 s, appel IA ~90 s, test ~180 s).
- **Cadence** : lance en série (`--workers=1`) les e2e qui font de VRAIS appels IA — en rafale, le fournisseur peut throttler (rate limit) et faire échouer des tests sains.
- **Sélecteurs précis** : `getByRole("heading", { name })` plutôt que `getByText` (qui matche aussi sous-titres/placeholders → strict mode violation). Si un nom court est le préfixe d'un nom plus long (« Commencer » vs « Commencer gratuitement »), utilise `{ exact: true }`.
- **Hygiène des données** : les e2e qui créent des comptes/données utilisent des identifiants uniques (préfixe + horodatage) et un `globalTeardown` qui purge ce préfixe — sinon la base se pollue à chaque run.
- **Hygiène** : tuer tout serveur dev fantôme sur le port avant un run ; purger `.next` si des chunks périmés traînent (« Cannot find module './XXX.js' »).

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
