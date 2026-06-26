---
name: ai-sdlc-planning
description: Phase 1 du AI-SDLC — Planification. Transforme une intention métier en specs exécutables (user stories, scénarios Gherkin, ADR, critères d'acceptation). À utiliser pour cadrer une nouvelle feature ou un projet AVANT d'écrire du code.
---

# Phase 1 — Planification (exécution Claude Code)
**Entrées :** une intention métier + `CLAUDE.md` (absent → `/init`). Tu ne codes RIEN.

## Étapes
1. Passe en mode plan (`Shift+Tab`).
2. Lève l'ambiguïté : liste ce qui est flou (périmètre, utilisateurs, volumétrie, contraintes, cas limites, **fournisseurs externes — IA, paiement, auth — à figer ICI** car en changer après coup est coûteux), pose les questions, attends les réponses.
3. Cadre : objectifs, non-objectifs explicites, capacités/features, risques majeurs.
4. Écris les user stories (« En tant que… je veux… afin de… »).
5. Écris les scénarios Gherkin par story (`Étant donné / Quand / Alors`) : nominal + erreur + cas limites.
6. Matérialise dans `specs/` : `specs/PRD.md` D'ABORD — **concis, droit à l'essentiel** (PAS de sections problème/opportunité/vision délayées) : utilisateurs cibles décrits **par l'expérience attendue** (pas de biographies), périmètre/non-goals, user stories, **écrans & interfaces décrits EN DÉTAIL** (par écran : layout, composants, contenu, états) + direction visuelle, et — **livrable clé — un prompt complet de génération de maquette** pour Claude Code (skill `frontend-design`). Évite de répéter une même contrainte (ex. mode de paiement) partout : une fois suffit. User stories : liste numérotée **extensive** (couvre tous les aspects). Décisions d'implémentation : modules, interfaces, contrats d'API, schéma — **sans chemins de fichiers ni extraits de code** (vite obsolètes). Puis `specs/README.md` (vision, modules, dépendances, glossaire) ; `specs/00X-<capacité>.md` (but, périmètre + non-goals, exigences Gherkin, contrats d'API, modèle de données, dépendances versions épinglées, critères d'acceptation vérifiables) ; `specs/adr/000X-<décision>.md` (contexte, options, décision, conséquences). Format : Markdown + YAML plat.
7. Découpe en lots indépendants et ordonne l'implémentation.
8. Route : par lot, complexité + modèle conseillé (Opus/Sonnet/Haiku) + posture (interactive vs déléguée).

## Porte de sortie
- [ ] `specs/PRD.md` relu et validé (problème, personas, objectifs/métriques, périmètre, risques).
- [ ] `specs/README.md` + une spec par capacité, critères d'acceptation vérifiables.
- [ ] Non-goals écrits.
- [ ] ADR pour chaque décision structurante.
- [ ] Découpage + ordre d'implémentation.
- [ ] Specs relues par un humain et commitées.

## Interdits
- Écrire du code.
- Laisser une ambiguïté « pour plus tard ».
- Spec qui ne couvre que le chemin heureux.
- Décision d'architecture sans ADR.

## Skills utiles (skill-finder)
Avant de coder a la main, verifie s'il existe une skill adaptee : invoque `skill-finder` (mots-cles : specs, gherkin, PRD, product brief). Installe seulement apres accord humain.
