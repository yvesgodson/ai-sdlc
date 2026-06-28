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

## Front-end (UI & landing de conversion)
Active toujours la skill `frontend-design` pour le rendu. Choisis une direction esthétique
**intentionnelle** (sobre/raffinée ou audacieuse) ; bannis les défauts d'IA (Inter/Arial,
dégradés violets sur blanc, layouts génériques).
- **Typo & couleur** : police distinctive, palette cohérente avec un accent net (tokens CSS).
- **Landing orientée conversion** (modèle éprouvé, cf. resumecoach.com), dans l'ordre :
  1. Hero : accroche bénéfice + promesse de temps (« en quelques minutes »), 2 CTA, aperçu produit.
  2. Fonctionnalités (3 max), avantages concrets.
  3. Comment ça marche : étapes numérotées.
  4. Pourquoi / bénéfices (grille).
  5. Preuve sociale UNIQUEMENT si réelle (avis, logos) — ne JAMAIS inventer chiffres/témoignages.
  6. FAQ (`<details>`/`<summary>` natif).
  7. CTA final + footer.
- **Copy minimal** : verbes d'action, orienté résultat, promesse de temps ; pas de superlatif mensonger. PAS de texte explicatif superflu dans l'UI — ne pas expliquer le fonctionnement interne (ex. « le texte est extrait puis structuré par l'IA… »). Microcopy courte ; au plus une ligne d'aide par champ.
- **Contrôles natifs stylés** : jamais le rendu navigateur par défaut. Un `input[type=file]` devient une zone de dépôt (drag & drop) custom, pas « Choose File ». Idem select/checkbox/radio.
- **Cohérence — chaque écran reçoit le MÊME soin** (pas seulement la landing) : en-tête de page uniforme (`h1` titre + sous-titre court `text-muted`), états vide/chargement/erreur soignés (icône + action), composants UI partagés réutilisés. Motifs récurrents à soigner : stepper multi-étapes, diff avant/après, barres de score, cartes cliquables (hover).
- **Tokens, JAMAIS la palette par défaut** : toute couleur — y compris les fonds sémantiques (succès, danger, info) — vient d'un token défini dans `@theme` (`text-ink`/`text-muted`, `bg-success-50`, `bg-danger-50`…). Ne JAMAIS piocher dans la palette Tailwind par défaut (`bg-green-50`, `text-gray-*`, `red-500`) : cela casse la discipline monochrome et introduit des teintes hors charte. Si un fond sémantique manque, ajoute le token (`--color-success-50`, `--color-danger-50`) plutôt que d'utiliser un défaut.
- **Factoriser les composants signature dès le 2e usage** : un élément de marque réutilisé (anneau de score, wordmark, badge de statut) devient un composant partagé à sa 2e apparition — ne pas copier-coller le balisage d'un écran à l'autre.
- **Support graphique — donner vie sans casser la sobriété** : une UI uniquement textuelle paraît vide. Ajoute des **illustrations et décors en SVG inline** (jamais d'emoji, jamais d'asset externe → offline, pas de dépendance) : illustrations line-art pour les panneaux d'accueil/auth, **états vides et chargements illustrés** (pas juste une icône), **iconographie distincte par concept** (une icône différente par fonctionnalité, pas le même check partout), accents discrets (étincelle SVG pour l'IA, taches de couleur diffuses `radial-gradient` floutées, motifs de points), petits éléments flottants qui racontent le produit (ex. chip « accroche réécrite »). Règles : tout reste **on-brand et monochrome** (la palette du projet prime, pas de couleurs criardes) ; tout décor est `aria-hidden` ; concentre la richesse graphique sur les surfaces d'entrée/marketing/états vides/chargement et garde les écrans de données denses calmes. Factorise ces graphiques dans un module dédié (`components/graphics`).
- **Animations** : reveals au défilement (IntersectionObserver) + entrées échelonnées au chargement
  (`animation-delay`) + micro-interactions sobres. TOUJOURS respecter `prefers-reduced-motion`.
- **Accessibilité WCAG AA** : labels associés, focus visible, contraste >= 4.5:1, clavier complet.

### Skills design — quand utiliser quoi
Trois skills se chevauchent sur le design : voici la hiérarchie pour ce projet (direction sobre, bleu monochrome).
- **`frontend-design` = générateur PRINCIPAL.** C'est lui qui produit le rendu, dans la direction du projet (sobre/raffinée, accent bleu). Toujours actif pour générer l'UI.
- **`high-end-visual-design` = inspiration premium, tempérée.** À piocher pour le RAFFINEMENT seulement : ombres diffuses, micro-interactions et physique des transitions (cubic-bezier custom), rythme d'espacement, typo distinctive (éviter Inter/Roboto/Arial). NE PAS basculer en maximalisme : pas d'OLED `#050505`, pas de glassmorphism `backdrop-blur` généralisé, pas de mesh gradients ni de rotations de cartes. La sobriété du projet prime sur les opinions « agence ».
- **`web-design-guidelines` = AUDIT, pas génération.** À lancer en REVUE du rendu (accessibilité/UX) une fois l'écran produit — pas pour créer l'UI. Outil de Phase 4 (cf. `ai-sdlc-test`).

## Porte de sortie
- [ ] ADR pour chaque décision structurante.
- [ ] Contrats API, schémas BDD, modèles de données figés dans `specs/`.
- [ ] Spec UI par écran (si UI).
- [ ] Bundle de handoff reçu (ou MCP Figma branché).
- [ ] Squelette échafaudé, versions épinglées.
- [ ] **Maquette : build de production vert** — `rm -rf .next && <build prod>` passe sans erreur de compilation, de types ni de lint, toutes les routes générées. C'est la porte de sortie de la maquette : on ne la déclare pas « prête » sans build vert.

## Interdits
- Décider l'architecture à la place de l'humain.
- Réinterpréter une maquette par capture au lieu du handoff/MCP.
- Rendu générique sans le design system.

## Skills utiles (skill-finder)
Avant de coder a la main, verifie s'il existe une skill adaptee : invoque `skill-finder` (mots-cles : design system, frontend-design, architecture, figma). Installe seulement apres accord humain.
