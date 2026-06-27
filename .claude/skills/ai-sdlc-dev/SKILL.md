---
name: ai-sdlc-dev
description: Phase 3 du AI-SDLC — Développement. Boucle par lot explore → plan → TDD → implémente → vérifie. À utiliser pour implémenter une feature à partir d'une spec validée.
---

# Phase 3 — Développement (exécution Claude Code)
**Entrées :** `specs/` + contrats figés + ADR ; bundle handoff UI si applicable.

## Boucle par lot
1. EXPLORER : lis `@specs/00X-...` et les fichiers concernés, ne modifie rien (subagent si volumineux).
2. PLANIFIER (mode plan) : plan fichier par fichier mappé aux critères ; attends l'OK humain.
3. TESTER D'ABORD : écris les tests dérivés des scénarios Gherkin (pas de test trivial).
4. IMPLÉMENTER : code jusqu'au vert, calque le style voisin, montre le diff après chaque fichier.
5. BOUCLER (bornée) : lance les tests, corrige la cause racine, relance. Arrêt = tout vert OU 5 itérations. Ne modifie jamais les tests pour les faire passer.
6. VÉRIFIER : résume le diff, mappe chaque changement à un critère, vérifie que chaque import correspond à un paquet réel.
7. COMMITTER : message Conventional Commits lié à la spec. Pas de commit sans accord.

## Parallélisation
Lots indépendants : 1 subagent par lot, en worktrees isolés si risque de conflit. Modèle adapté (Opus/Sonnet/Haiku).

## MCP
Consomme un serveur MCP existant plutôt qu'un wrapper. Audite-le avant accès ; credentials via variables d'environnement ; pas de données de prod ; lecture seule par défaut ; montre les entrées avant un appel sensible.

## Porte de sortie
- [ ] Code de chaque lot conforme aux contrats.
- [ ] Tests dérivés des scénarios, au vert.
- [ ] Diffs relus par un humain.
- [ ] Imports vérifiés (aucun paquet halluciné).
- [ ] Commits propres liés aux specs.

## Interdits
- Faire confiance aux 80 % « qui ont l'air bons » sans vérifier limites/erreurs.
- Gros diff inauditable.
- Modifier les tests pour les faire passer.
- Empiler des correctifs sur une mauvaise base → `/rewind` et reformuler.

## Backend / API
Pour les routes, API et services Node (App Router, route handlers), applique la skill `nodejs-backend-patterns` : validation des entrées par Zod, classes d'erreur dédiées, rate-limit, CORS non-`*`, health checks, connection pooling. Cohérent avec nos règles (pas de secret en clair, variables d'environnement, vérifier les imports).

## Frontières runtime & dépendances
- **Client / serveur / edge** : la validation (Zod) et les types vont dans un module *client-safe* (aucun import serveur) ; la logique serveur (Prisma, bcrypt, secrets, `next/headers`) reste dans des modules importés uniquement côté serveur. Le **`middleware` tourne en edge** : il n'importe QUE des modules edge-safe (`jose` oui ; JAMAIS `next/headers`, Prisma, bcrypt, ni `server-only`). Garde les constantes et la crypto partagées (nom de cookie, signature de jeton) dans un module edge-safe distinct du module qui touche `next/headers` — sinon le build casse. Un import serveur tiré dans un composant client OU un import `next/headers` tiré dans le middleware sont les deux pièges recurrents.
- **Dépendances** : avant d'épingler une version, vérifie ce qui est réellement publié (`npm view <pkg> version`) et choisis la majeure que tu sais supporter — ne recopie pas un numéro de mémoire (les majeures derivent : Prisma, zod, vitest…). N'ajoute une dépendance que **quand le lot courant l'utilise** (différer les autres) : install plus fiable, surface réduite. Avec Prisma, `prisma generate` doit précéder le build (script `build`).
- **Effets de bord à l'import** : certaines libs exécutent du code au `require`/`import` (ex. `pdf-parse` v1 lit un PDF de test si `module.parent` est absent → crash sous bundler/vitest). Si une lib casse à l'import, importe son sous-module interne (`pdf-parse/lib/pdf-parse.js`) + une déclaration de types, plutôt que de contourner par un mock.

## IA (sorties structurées)
Pour toute sortie IA exploitée par le code (structuration, extraction, scoring), triple défense :
1. **Prompt anti-hallucination** explicite : « n'invente rien, n'utilise que le texte fourni, laisse vide ce qui est absent ».
2. **Sortie structurée du SDK** (`responseSchema` + `responseMimeType: application/json` pour Gemini) pour contraindre la forme.
3. **Validation/normalisation Zod tolérante** côté serveur (champs manquants → valeurs vides) : ne jamais faire confiance au JSON du modèle tel quel.
Isole l'appel derrière une **fonction de service** (`src/lib/ai/`) pour localiser un futur changement de fournisseur. Clé via variable d'environnement ; en prod, ne définir QUE la clé voulue (les SDK lisent parfois plusieurs noms, ex. `GOOGLE_API_KEY` vs `GEMINI_API_KEY`). Teste la partie pure (prompt/parse) en unitaire ; garde l'appel réseau dans un test d'intégration *gated* par la présence de la clé (ignoré sinon).
- **Éditions IA appliquées au contenu** : quand l'IA propose des réécritures à appliquer, cible par **correspondance exacte du texte** (`before`/`after`), pas par index (l'index hallucine) ; rends la fonction d'application pure et testable. Laisse l'utilisateur **accepter partiellement**. Après édition, **re-score le contenu réel** (nouvel appel) plutôt que d'afficher un score projeté fictif — un score doit toujours refléter l'état courant.

## Skills utiles (skill-finder)
Avant de coder a la main, verifie s'il existe une skill adaptee : invoque `skill-finder` (mots-cles : testing, TDD, code-review, refactor, debugging). Installe seulement apres accord humain.
