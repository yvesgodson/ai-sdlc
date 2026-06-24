---
name: doc-vivante
description: |
  Met à jour le README et le CHANGELOG à partir des commits depuis la dernière release.
  À invoquer avec /doc-vivante. À utiliser après une série de changements committés ou
  avant une release. NE PAS réécrire la doc de zéro : on incrémente.
disable-model-invocation: true
allowed-tools: Read, Edit, Bash(git log *), Bash(git tag *), Bash(git diff *)
---

# Documentation vivante (README + CHANGELOG)

## Quand l'utiliser
- Après une série de commits, ou juste avant une release.

## Démarche
1. Déterminer la dernière release (`git tag --sort=-creatordate`) et lister les commits
   depuis (`git log <dernier-tag>..HEAD`).
2. Regrouper par type (feat / fix / refactor / docs / breaking) — format Conventional Commits.
3. Mettre à jour `CHANGELOG.md` (section `[Unreleased]` ou la prochaine version) **sans toucher
   l'historique existant**.
4. Mettre à jour les sections impactées du `README` (installation, usage, API) **uniquement
   si un commit les concerne**.
5. Signaler les **breaking changes** en tête de la section.

## Format de sortie
- Un résumé des entrées ajoutées au CHANGELOG.
- La liste des sections du README modifiées (avec justification = commit source).

## Anti-patterns
- Ne pas inventer d'entrées : se baser sur les commits réels.
- Ne pas réécrire tout le README ; édition chirurgicale.
- Ne pas committer soi-même : proposer, l'humain valide.
