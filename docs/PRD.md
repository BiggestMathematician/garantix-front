# PRD — Garantix Front

## Le problème
Une garantie est une promesse avec une date de péremption. Personne ne compte les deux, trois ou cinq ans : le ticket jaunit et le droit s'évapore sans qu'on le sache.

## L'app en une phrase
Garantix tient le registre de ce que vous possédez et vous dit, au jour près, ce qui est encore couvert.

## Contexte
Version front-end, sans backend, de Garantix (app Next.js + Prisma existante, dépôt séparé). Les achats viennent d'un fichier JSON de démonstration (12 à 20 achats fictifs) ; les favoris et les achats ajoutés sont gardés dans le `localStorage`.

## Vocabulaire
| Terme | Définition |
|---|---|
| Achat | Un objet possédé : produit, marque, catégorie, vendeur, prix, date d'achat, durée de garantie en années |
| Échéance | Date d'achat + N années civiles (même jour, même mois) |
| Statut | Calculé, jamais stocké : **Active** (> 30 jours restants), **Bientôt expirée** (0 à 30 jours), **Expirée** (< 0) |
| Suivi | Un achat mis en favori, pour le garder à l'œil |

## Type Schema.org
`Product`, avec `offers` (`Offer` : prix, devise, vendeur) et `offers.warranty` (`WarrantyPromise` : `durationOfWarranty`).

## Exigences

| ID | Exigence | Critères d'acceptation |
|---|---|---|
| EX-01 | Voir la liste | La page d'accueil (le coffre) affiche les achats du fichier JSON et ceux ajoutés par l'utilisateur, chacun avec produit, marque, catégorie, échéance et statut. Si la liste est vide, un message clair invite à enregistrer un premier achat. |
| EX-02 | Filtrer | Trois filtres combinables : catégorie, statut de garantie, recherche texte (produit, marque, vendeur). Le nombre de résultats est annoncé aux lecteurs d'écran (`aria-live="polite"`). Un filtre sans résultat affiche un message et un moyen de réinitialiser. |
| EX-03 | Voir le détail | Chaque achat a sa page `/achats/:slug`, partageable. Elle affiche la jauge de garantie (Rosette), l'échéance, les jours restants ou dépassés, le vendeur et le prix. Titre de page et meta description propres à l'achat. Un slug inconnu affiche une page « Introuvable ». |
| EX-04 | Garder ses favoris | Un bouton « Suivre » sur la carte et le détail ajoute ou retire l'achat des suivis. Les suivis restent après rechargement. Un compteur dans l'en-tête affiche leur nombre et se met à jour immédiatement. |
| EX-05 | Proposer un élément | Formulaire « Enregistrer un achat » : produit, marque, catégorie, date d'achat, durée de garantie (obligatoires), vendeur et prix (facultatifs). Validation accessible : messages liés aux champs (`aria-describedby`), `aria-invalid`, focus sur le premier champ en erreur. L'achat apparaît dans le coffre et reste après rechargement. |
| EX-06 | S'adapter à l'écran | Utilisable sans défilement horizontal de 320 px à 1440 px de large. |
| EX-07 | Être en ligne | L'app est publiée sur GitHub Pages ; le README donne son adresse. |

## Règles de validation (EX-05)
- Produit : 2 à 80 caractères.
- Marque : 1 à 40 caractères.
- Catégorie : une des catégories du catalogue.
- Date d'achat : valide, pas dans le futur.
- Durée : entier de 1 à 10 ans.
- Prix : facultatif, nombre ≥ 0, deux décimales maximum.

## Pour aller plus loin (si en avance)
- Mode sombre construit sur les tokens.
- Page « Mes suivis ».
- Tri : échéance la plus proche, date d'achat, prix.

## Hors périmètre
Authentification, backend, upload de preuve d'achat, dossiers SAV, i18n, modification ou suppression d'un achat du catalogue JSON.
