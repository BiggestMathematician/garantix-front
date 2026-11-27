# Garantix Front — instructions agent

App web sans backend qui tient un coffre de garanties : liste d'achats, statut de garantie calculé, favoris, ajout d'un achat.

## À lire avant d'écrire du code
- `docs/PRD.md` : exigences EX-01 à EX-07 et critères d'acceptation.
- `docs/TRD.md` : stack, architecture, ADR. Ne pas contredire un ADR sans en écrire un nouveau.
- `docs/DESIGN_SYSTEM.md` : tokens et composants.
- `docs/IMPLEMENTATION_PLAN.md` : lot en cours et Definition of Done.

## Règles
- Pas de backend, pas d'API, pas de base de données. Données : `src/data/achats.json`. Persistance : `localStorage` uniquement.
- TypeScript strict. Aucun `any` ; si inévitable, commentaire `// any: <raison>` sur la même ligne.
- Aucune valeur en dur dans le CSS : couleurs, espacements, tailles, rayons, durées viennent de `src/styles/tokens.css`.
- Tout composant de `src/components/` a sa story Storybook.
- HTML sémantique d'abord ; ARIA seulement quand le HTML ne suffit pas. Tout est utilisable au clavier.
- Le domaine (`src/domain/`) est pur : pas de React, pas de DOM, pas de `localStorage`. La date du jour est un paramètre.
- Un lot à la fois, plan validé avant le code. Ne rien ajouter hors du lot en cours.

## Commits
- Très petits : un changement = un commit.
- Format : `type(portée): message court [EX-0x]`. L'exigence est citée quand le commit en sert une.
- Jamais de ligne `Co-Authored-By`.

## Commandes
- `npm run dev` · `npm run build` · `npm run test` · `npm run lint` · `npm run storybook`
