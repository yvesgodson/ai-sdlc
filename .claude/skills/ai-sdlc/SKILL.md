---
name: ai-sdlc
description: Processus global AI-SDLC pour Claude Code — pilote l'enchaînement des 6 phases (planification, design, développement, test, déploiement, maintenance) avec des portes de validation. À utiliser au démarrage d'un projet, pour savoir quelle phase appliquer, ou pour orchestrer le cycle de bout en bout.
---

# Processus global AI-SDLC — Instructions pour Claude Code

Tes instructions d'exécution. Quand une phase démarre, charge son fichier (`01`→`06`) et applique ses étapes dans l'ordre.

## Posture permanente
- Ne code jamais sans spec ni plan validés. Si l'intention est ambiguë, pose des questions et attends la réponse.
- Énonce le critère vérifiable avant d'agir ; prouve-le après (test, commande, sortie).
- Travaille en petits lots auditables. Montre le diff après chaque fichier.
- Vérifie, ne fais pas confiance. Relis chaque ligne ; vérifie que chaque import correspond à un paquet réel.
- Ne modifie jamais tests et implémentation dans le même tour. Les tests ne changent que sur décision humaine.
- Respecte hooks et permissions. Aucun secret en clair. Aucune action prod sans approbation humaine.
- À chaque erreur corrigée par l'humain, propose une règle (`CLAUDE.md`) ou une skill.

## Pré-vol (début de session)
1. Lire `CLAUDE.md` (auto). Absent → proposer `/init`.
2. Identifier la phase courante (planif, design, dev, test, déploiement, maintenance).
3. Charger la spec : `@specs/<id>-<capacité>.md`. Absente hors planif → signaler et proposer de la créer.
4. `/context`. Saturé → `/compact` ou `/clear`.
5. Ouvrir `modules/0X-<phase>.md` et appliquer ses étapes.

## Machine à états des phases
```
[Planification] → specs validées + commitées (porte : relues par un humain)
[Design]        → ADR + contrats figés + handoff UI
[Développement] → code + tests verts, diffs relus
[Test & QA]     → tests + evals au seuil (7 dimensions)
[Déploiement]   → revue, hooks actifs, CI verte, release progressive
[Maintenance]   → observabilité, corrections minées → remonte en [Planification]
```
Ne passe à la phase suivante que si la porte courante est franchie. Sinon, reste et signale ce qui bloque.

## Boucle d'exécution (dans chaque phase)
1. EXPLORER : lire fichiers/spec, ne rien modifier (déléguer à un subagent si volumineux).
2. PLANIFIER : plan étape par étape mappé aux critères ; attendre l'OK humain.
3. TESTER D'ABORD : écrire les tests dérivés des scénarios.
4. IMPLÉMENTER : coder jusqu'au vert ; diff après chaque fichier.
5. BOUCLER : tests/lint, corriger la cause racine, relancer. Arrêt = tout vert OU N itérations max.
6. VÉRIFIER : mapper le diff aux critères ; signaler tout écart.
7. RENDRE : proposer le commit (Conventional Commits). Pas de commit sans accord si effet de bord.

## Critères d'arrêt
Arrête-toi et demande une décision humaine si :
- l'intention est ambiguë ou contredit la spec ;
- l'action est irréversible ou touche la prod ;
- une boucle atteint son plafond sans converger ;
- un hook bloque une action ;
- tu vas modifier des tests, supprimer du code non créé par la session, ou installer une dépendance non épinglée.

## Outils — skill-finder
A chaque phase, si un besoin d'outillage emerge (UI, tests, CI, doc...), invoque la skill `skill-finder` pour chercher une skill existante sur skills.sh (leaderboard + `npx skills find`). Montre-la et demande l'accord avant `npx skills add`.
