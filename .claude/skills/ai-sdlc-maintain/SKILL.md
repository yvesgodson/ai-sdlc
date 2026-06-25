---
name: ai-sdlc-maintain
description: Phase 6 du AI-SDLC — Maintenance. Débogage par les preuves, migration à l'échelle, observabilité, documentation vivante. À utiliser pour maintenir ou faire évoluer un système en production.
---

# Phase 6 — Maintenance (exécution Claude Code)
**Entrées :** système en production, logs, traces, retours utilisateurs.

## Procédures
**A. Débogage**
1. Récupère la preuve : log d'erreur complet, commande qui échoue.
2. Écris d'abord un test qui reproduit le bug.
3. Corrige la cause racine, pas le symptôme. Tout renommage = tâche séparée.
4. Garde le test de repro (anti-régression).

**B. Legacy** — délègue à un subagent : « cartographie `src/<module>/` → responsabilités, points d'entrée, dépendances, dette, risques. Ne rien modifier. »

**C. Migration à l'échelle**
1. Écris une spec de migration (`specs/`) : avant/après, règles de transformation, cas à ne pas migrer.
2. Par unité : 1 subagent migre en worktree isolé → tests → 1 subagent juge la conformité.
3. Tests verts avant l'unité suivante. Borne et journalise ce qui n'a pu être migré.

**D. Amélioration continue** — mine les corrections utilisateurs : corrige la source (`CLAUDE.md`, spec ou skill), pas l'instance. À chaque incident récurrent : propose une règle, une skill ou un hook.

**E. Documentation vivante** — mets à jour README/CHANGELOG depuis les commits. Édition chirurgicale, n'invente pas d'entrées.

## Observabilité (avant l'incident)
Traces (durée de session, cycles de raisonnement, appels d'outils + latences), métriques (coût tokens, latence, boucles d'auto-réparation), échantillonnage tail-based. Surveille : dérive d'intention, coût qui dérape, ratio auto-réparation.

## Sécurité continue
Trace d'audit immuable (action ↔ agent ↔ humain ayant approuvé), disjoncteurs (rollback + révocation des accès), re-scan SCA périodique, hooks de garde toujours actifs.

## Porte de sortie
- [ ] Observabilité active (traces, coût, dérive).
- [ ] Débogage par les preuves (test de repro avant correction).
- [ ] Spec de migration pour la modernisation.
- [ ] Corrections minées → source corrigée.
- [ ] Trace d'audit + disjoncteurs.
- [ ] Re-scan SCA périodique.

## Interdits
- Corriger un symptôme sans test de repro.
- Migration « big bang » sans pipeline ni tests par étape.
- Corriger l'instance sans corriger la cause.
- Tronquer silencieusement une migration.

## Skills utiles (skill-finder)
Avant de coder a la main, verifie s'il existe une skill adaptee : invoque `skill-finder` (mots-cles : observability, monitoring, documentation, migration). Installe seulement apres accord humain.
