---
name: ai-sdlc-design
description: Phase 2 du AI-SDLC — Design. Architecture, ADR, contrats d'API, et migration Claude Design → Claude Code (handoff UI, MCP Figma). À utiliser pour concevoir l'architecture ou transformer une maquette en code.
---

# Phase 2 — Design (exécution Claude Code)
**Entrées :** `specs/` validées (phase 1).

## Étapes
1. Mode plan : propose 2–3 architectures candidates depuis `@specs/` (composants, flux, trade-offs, risques, coût). Ne code rien.
2. Fais trancher l'humain ; n'impose pas l'architecture.
3. Trace la décision : `specs/adr/000X-<décision>.md`.
4. Fige les contrats dans `specs/` : API, schémas BDD, modèles de données, conventions de nommage.
5. Échafaude la structure conforme aux ADR/contrats. Épingle les versions. Squelette des modules sans logique métier.

## Handoff UI (si interface)
1. Source = Claude Design (`claude.ai/design`), design system de l'org importé. N'invente pas l'UI.
2. Maquette validée → « Send to Claude Code » : lis nativement le bundle (structure, tokens, composants, layout). Ne demande pas de capture à réinterpréter.
3. Variante Figma : `claude mcp add figma -- <commande du serveur MCP Figma>`, puis lis composants/tokens/structure.
4. Lie une spec UI `specs/ui/<page>.md` : états (hover, erreur, vide, chargement), responsive, accessibilité (WCAG AA).
5. Implémente avec le design system + style des composants existants. Tests visuels d'abord (Playwright + comparaison de captures), puis le code.
6. Active la skill `frontend-design` pour un rendu de qualité.

## Porte de sortie
- [ ] ADR pour chaque décision structurante.
- [ ] Contrats API, schémas BDD, modèles de données figés dans `specs/`.
- [ ] Spec UI par écran (si UI).
- [ ] Bundle de handoff reçu (ou MCP Figma branché).
- [ ] Squelette échafaudé, versions épinglées.

## Interdits
- Décider l'architecture à la place de l'humain.
- Réinterpréter une maquette par capture au lieu du handoff/MCP.
- Rendu générique sans le design system.
