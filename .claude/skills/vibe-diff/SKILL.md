---
name: vibe-diff
description: |
  Traduit le diff courant en résumé clair en français et MAPPE chaque changement aux
  critères d'acceptation de la spec liée, avant une fusion ou une action à fort enjeu.
  À invoquer manuellement avec /vibe-diff. Signale tout changement non justifié par la spec.
disable-model-invocation: true
allowed-tools: Read, Grep, Bash(git diff *), Bash(git status *), Bash(git log *)
---

# Vibe Diff — revue d'intention avant fusion

## Objectif
Avant une fusion ou une action à fort enjeu, confirmer l'**alignement d'intention**
(et pas seulement « ça compile »), en traduisant le code en langage clair.

## Démarche
1. Récupérer le diff (`git diff`, `git status`, au besoin `git log`).
2. Lire la/les spec(s) liée(s) dans `specs/` (demander laquelle si ambigu).
3. Pour CHAQUE changement, produire une ligne :
   `fichier → ce que ça fait → critère d'acceptation satisfait`
   (ou « ⚠️ aucun critère : à justifier »).
4. Lever des **alertes dédiées** :
   - Imports vers des paquets potentiellement **hallucinés** (à vérifier sur le registre).
   - Tests **supprimés, mockés ou affaiblis**.
   - **Secrets** / clés / mots de passe en clair.
   - Changements **hors périmètre** de la spec.
5. Conclure : « ✅ Alignement OK » ou « ⛔ Points à clarifier avant fusion » (liste).

## Format de sortie
- Un résumé en français de ce que fait réellement le diff.
- Un tableau `changement → critère d'acceptation`.
- Une section « Alertes » (vide si rien).

## Anti-patterns
- NE JAMAIS committer/fusionner soi-même : on informe, l'humain décide.
- Ne pas se contenter de paraphraser le code : relier à l'INTENTION (la spec).
- Ne pas masquer une alerte parce que « ça a l'air correct ».
