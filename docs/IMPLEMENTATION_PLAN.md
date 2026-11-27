# Plan d'implémentation — Garantix Front

Ordre guidé par la priorité du sujet : liste, détail et mise en ligne d'abord ; filtres et bonus en dernier. Un lot = une conversation avec l'agent, un plan validé, des petits commits.

## Definition of Done (chaque lot)
- [ ] `npm run build`, `npm run lint` et `npm run test` passent.
- [ ] Aucun `any` non justifié, aucune valeur CSS en dur.
- [ ] Composant nouveau ou modifié → story à jour.
- [ ] Parcours du lot fait au clavier seul (Tab, Maj+Tab, Entrée, Espace, Échap).
- [ ] Vérifié à 320 px et à 1440 px, sans défilement horizontal.
- [ ] Commits qui citent l'exigence servie.
- [ ] Docs mises à jour si une décision a changé.

## Lots

| Lot | Contenu | Exigences | Date |
|---|---|---|---|
| 0 | Vite + React + TS strict, ESLint, Stylelint, `tokens.css`, Storybook qui démarre, `Button` et `StatusBadge` avec stories, `achats.json` (16 achats) et types, domaine garantie porté de Garantix avec tests | — | 27/11 |
| 1 | Page Coffre : `AchatCard`, `EmptyState`, grille responsive, en-tête et pied de page ; workflow GitHub Pages, adresse dans le README | EX-01, EX-07 | 27/11 si possible, sinon 04/12 |
| 2 | Routing, page Détail avec `Rosette`, page Introuvable, titre et description par page | EX-03 | 04/12 |
| 3 | Passe responsive et accessibilité sur Coffre et Détail ; lien d'évitement, focus visible, landmarks | EX-06 | 04/12 |
| 4 | `AchatRepository` (pattern Repository), `Field`, formulaire Enregistrer avec validation accessible | EX-05 | 04/12 |
| 5 | `SuivisStore` (pattern Observer), `FollowToggle`, compteur dans l'en-tête | EX-04 | 04/12 |
| 6 | `FilterBar` : catégorie + statut + recherche, compteur `aria-live` | EX-02 | 04/12 |
| 7 | Pré-rendu statique des routes, JSON-LD `Product` / `Offer` / `WarrantyPromise`, `404.html` | SEO, GEO | 04/12 |
| 8 | Audits : EcoIndex avant et après, RGAA du parcours « trouver un achat → le suivre → enregistrer un achat », Lighthouse, validation Schema.org ; corrections | Audits | 04/12 → 11/12 |
| 9 | Bonus : mode sombre, page Mes suivis, tri | Bonus | si en avance |
| — | Support de soutenance, gel du code | — | 11/12, 12h30 |

## Si je prends du retard
Couper dans cet ordre : lot 9, puis lot 6 réduit à deux filtres, puis lot 7 réduit au JSON-LD rendu côté client. Ne jamais couper le lot 8 : `docs/AUDITS.md`, même incomplet, compte dans la grille.

## Journal de l'agent
Ce que l'agent a raté et la règle ajoutée en réponse (sert à la soutenance, partie 5).

| Date | Ce qui a raté | Règle ajoutée |
|---|---|---|
| | | |
