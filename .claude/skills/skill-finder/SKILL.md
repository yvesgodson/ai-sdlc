---
name: skill-finder
description: Trouve et installe des skills Claude Code adaptees a un besoin ou a une etape du AI-SDLC, depuis l'annuaire skills.sh. A utiliser quand l'utilisateur demande « existe-t-il une skill pour… », « comment faire X », « trouve-moi une skill », « peux-tu… », ou quand une phase du AI-SDLC (planification, design, developpement, test, deploiement, maintenance) gagnerait a etre outillee par une skill existante.
---

# Skill Finder — trouver la bonne skill sur skills.sh

Inspire de la skill `find-skills` (vercel-labs). Principe : reperer une skill
existante eprouvee plutot que reinventer, a chaque etape du AI-SDLC. Tu proposes,
l'humain decide ; tu n'installes jamais sans accord.

## Mecanisme (skills.sh)
1. **Leaderboard** : consulter https://skills.sh/ (skills classees par installations) AVANT les recherches CLI.
2. **Recherche CLI** : `npx skills find "<requete>" [--owner <owner>]`.
3. **Installation** (apres accord) : `npx skills add <owner/repo@skill> -g -y`
   (globale, confirmation auto) — ou sans `-g` pour l'installer dans le projet courant.

## Procedure
1. **Cerner le besoin** : domaine + tache + phase AI-SDLC concernee.
2. **Leaderboard d'abord** : verifier les skills populaires deja eprouvees.
3. **Recherche ciblee** : lancer `npx skills find` avec des mots-cles precis (table ci-dessous).
4. **Valider la qualite** : nombre d'installations (privilegier 1k+), reputation de la source,
   etoiles GitHub. Ecarter les skills douteuses ou non maintenues.
5. **Presenter 1 a 3 options** : nom, ce qu'elle fait, nb d'installations, commande
   d'installation, lien `https://skills.sh/<owner>/<repo>/<skill>`.
6. **Installer seulement apres accord humain.** Montrer le contenu de la skill avant.

## Mots-cles par etape du AI-SDLC
| Phase | Rechercher (mots-cles) |
|---|---|
| Planification | specs, user stories, gherkin, PRD, product brief |
| Design | design system, frontend-design, architecture, figma, API design |
| Developpement | testing, TDD, code-review, refactor, debugging, `<techno>` |
| Test & QA | evals, LLM-as-judge, playwright, accessibility, security audit |
| Deploiement | CI/CD, vercel, docker, release, changelog |
| Maintenance | observability, monitoring, documentation, migration |

Skills utilitaires de reference a connaitre : `frontend-design`, `skill-creator`,
`code-review`, `deep-research`, et les skills « document » (docx/pdf/pptx).

## Skills deja installees dans ce projet
Inutile de les rechercher sur skills.sh, elles sont deja presentes dans `.claude/skills/` :
- `frontend-design` — generateur UI PRINCIPAL (rendu sobre, bleu monochrome).
- `high-end-visual-design` — inspiration premium (ombres, micro-interactions, typo), a temperer par la sobriete du projet.
- `web-design-guidelines` — AUDIT du rendu UI/accessibilite (revue, Phase 4), pas un generateur.
- `nodejs-backend-patterns` — bonnes pratiques backend Node (Zod, classes d'erreur, rate-limit, CORS, health checks).

## Interdits
- Installer une skill sans accord humain explicite.
- Recommander une skill sans avoir verifie son usage et sa reputation (installs/etoiles).
- Reinventer a la main ce qu'une skill eprouvee fait deja mieux.
