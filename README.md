# AI-SDLC — WAOUHMONDE

Un **cycle de développement logiciel piloté par l'IA**, outillé pour **Claude Code** sous forme de **skills** (format standard `SKILL.md`).

L'idée : passer du *vibe coding* (« je décris, j'accepte ») à une ingénierie disciplinée où l'IA produit du code **à l'intérieur de contraintes, de tests et de boucles de vérification que vous concevez**.

## Contenu du dépôt
```
.claude/skills/      # les skills (une par phase + orchestrateur + skills maison)
Cours-AI-SDLC-WAOUHMONDE.docx   # le guide pratique (pour les développeurs)
docs/generate-docs.md           # le brief d'origine
```

## Les skills

| Skill | Rôle |
|---|---|
| `ai-sdlc` | Orchestrateur : pilote l'enchaînement des 6 phases avec portes de validation |
| `ai-sdlc-planning` | Phase 1 — intention → specs exécutables (Gherkin, ADR, critères) |
| `ai-sdlc-design` | Phase 2 — architecture, contrats, migration Claude Design → Claude Code |
| `ai-sdlc-dev` | Phase 3 — boucle explore → plan → TDD → implémente → vérifie |
| `ai-sdlc-test` | Phase 4 — tests + evals (LLM-as-judge), 7 dimensions, revue par subagents |
| `ai-sdlc-deploy` | Phase 5 — revue, hooks de sécurité, CI/CD, release progressive |
| `ai-sdlc-maintain` | Phase 6 — débogage par preuves, migration, observabilité, doc vivante |
| `cadrage-spec` | Rédige une spec au format maison dans `specs/` |
| `vibe-diff` | Revue d'intention avant fusion : mappe le diff aux critères d'acceptation |
| `doc-vivante` | Met à jour README/CHANGELOG depuis les commits |

## Utilisation avec Claude Code

Le dossier `.claude/skills/` est détecté automatiquement à l'ouverture du dépôt.
- Invoquez une skill manuellement : `/ai-sdlc`, `/ai-sdlc-planning`, …
- Ou laissez Claude la charger selon le contexte (la `description` de chaque skill sert de déclencheur).

Pour réutiliser ces skills dans un autre projet, copiez les dossiers voulus dans le `.claude/skills/` de ce projet (ou ajoutez-les via l'annuaire de skills).

## Le guide pratique

Ouvrez **`Cours-AI-SDLC-WAOUHMONDE.docx`** : le AI-SDLC pas à pas, avec pour chaque étape des encarts courts sur les **tokens**, les **commandes utiles** et les **subagents & loops**.

> *Generation is solved. Verification, judgment, and direction are the new craft.*
