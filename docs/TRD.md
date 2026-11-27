# TRD — Garantix Front

## Stack
| Rôle | Choix |
|---|---|
| Build et dev | Vite |
| Langage | TypeScript, `strict: true`, `noUncheckedIndexedAccess: true` |
| UI | React 19 |
| Routing | React Router (mode déclaratif) |
| Styles | CSS Modules + custom properties (`src/styles/tokens.css`) |
| Documentation composants | Storybook 10 (React + Vite), addons a11y, docs et MCP |
| Tests | Vitest (domaine et stores) |
| Qualité | oxlint (fourni par le gabarit Vite, plugins react et jsx-a11y), Stylelint (interdit les valeurs en dur hors tokens) |
| Hébergement | GitHub Pages, construit par GitHub Actions |
| Node | ≥ 22 LTS (Vite et Storybook récents exigent ≥ 20.19) |

## Architecture
```
src/
  domain/       règles pures : types, calcul d'échéance et de statut, validation
  data/         achats.json (catalogue de démonstration)
  services/     accès aux données : repository, store des suivis
  components/   composants du design system, chacun avec sa story
  pages/        Coffre, Détail, Enregistrer, Introuvable
  styles/       tokens.css, base.css
```
Sens des dépendances : `pages → components → services → domain`. Le domaine ne dépend de rien.

## Types du domaine
```ts
type Categorie = "electromenager" | "informatique" | "telephonie" | "audio" | "mobilite" | "maison";
type StatutGarantie = "ACTIVE" | "BIENTOT_EXPIREE" | "EXPIREE";

interface Achat {
  slug: string;
  produit: string;
  marque: string;
  categorie: Categorie;
  vendeur: string | null;
  prixCentimes: number | null;
  dateAchat: string;          // ISO AAAA-MM-JJ
  dureeGarantieAns: number;
  description: string;
  origine: "catalogue" | "utilisateur";
}

interface EtatGarantie {
  statut: StatutGarantie;
  echeance: string;
  joursRestants: number;      // négatif si expirée
  ratio: number;              // part de garantie restante, 0 à 1
}
```
Le statut n'est jamais stocké : il est calculé avec la date du jour passée en paramètre (testable, et juste quel que soit le jour de la démo).

## Design patterns

### Repository — `AchatRepository`
**Problème** : la liste mélange deux sources, le JSON en lecture seule (EX-01) et les achats ajoutés dans le `localStorage` (EX-05). Les pages ne doivent pas savoir d'où vient un achat.
**Solution** : une interface `AchatRepository { lister(): Achat[]; trouver(slug): Achat | undefined; ajouter(saisie): Achat }`, implémentée par `CatalogueJson` (lecture seule) et `StockageLocal` (`localStorage`), réunies par `CoffreRepository` qui les combine. Changer de source (une vraie API un jour) ne touche que cette couche.

### Observer — `SuivisStore`
**Problème** : le compteur de l'en-tête, les cartes et la page détail affichent les suivis (EX-04) ; ils doivent rester synchronisés sans se passer l'état de composant en composant, y compris entre deux onglets.
**Solution** : une classe `SuivisStore` (sujet) avec `subscribe(listener)`, `getSnapshot()`, `basculer(slug)`. Elle persiste dans le `localStorage` et notifie ses abonnés ; elle écoute l'événement `storage` pour les autres onglets. React s'y abonne avec `useSyncExternalStore`, l'API prévue pour les stores externes.

## Clés localStorage
| Clé | Contenu |
|---|---|
| `garantix:suivis` | `string[]` de slugs |
| `garantix:achats` | `Achat[]` ajoutés par l'utilisateur |
| `garantix:theme` | `"clair" \| "sombre"` (bonus) |

Toute lecture est validée (JSON invalide ou forme inattendue → valeur vide), jamais un `as` aveugle.

## ADR

### ADR-001 — React plutôt que vanilla ou Vue
- **Contexte** : l'app a des états partagés (filtres, suivis, formulaire) et une dizaine de composants à documenter dans Storybook. Garantix a déjà un front React (jauge Rosette, géométrie guilloché, calcul de garantie).
- **Décision** : React 19 avec TypeScript.
- **Raisons** : réutiliser mon propre code existant (cité dans le README) ; rendu déclaratif qui évite la synchronisation DOM à la main ; `useSyncExternalStore` pour brancher le pattern Observer proprement ; Storybook a son support le plus mûr pour React ; React 19 rend `<title>` et `<meta>` depuis les composants (EX-03).
- **Conséquences** : ~60 ko gzip de runtime, compensés par le pré-rendu (ADR-003). Vanilla aurait été plus léger mais m'aurait fait réécrire routing, rendu et état ; Vue n'aurait rien permis de réutiliser.

### ADR-002 — CSS Modules et tokens en custom properties, pas de framework CSS
- **Décision** : un fichier `tokens.css` (primitives puis sémantiques), des CSS Modules par composant.
- **Raisons** : « aucune valeur en dur » vérifiable par Stylelint ; le mode sombre consiste à redéfinir les tokens sémantiques ; zéro dépendance de style, CSS généré minimal.

### ADR-003 — Pré-rendu statique des pages au build
- **Contexte** : GitHub Pages ne sert que des fichiers statiques. Une SPA pure renvoie une page vide aux robots qui n'exécutent pas JavaScript (une partie des moteurs et la plupart des crawlers d'IA, donc SEO et GEO), et une erreur 404 sur un lien profond `/achats/:slug`.
- **Décision** : script de build qui rend chaque route du catalogue en HTML (`react-dom/server`), avec titre, description et JSON-LD dans le HTML ; l'app s'hydrate ensuite. `404.html` sert de repli SPA pour les achats ajoutés par l'utilisateur.
- **Conséquences** : un HTML par achat, liens profonds partageables, contenu lisible sans JavaScript. Un peu plus de complexité de build, documentée dans le README.

### ADR-004 — Données importées au build, pas récupérées à l'exécution
- **Décision** : `achats.json` est importé par le module, donc inclus dans le bundle.
- **Raisons** : zéro requête réseau supplémentaire (éco-conception), pas d'état de chargement ni d'erreur réseau à gérer. Viable pour 12 à 20 éléments.

## Déploiement
`vite.config.ts` : `base: '/garantix-front/'`. Workflow `.github/workflows/deploy.yml` (guide officiel Vite) déclenché sur `main`. Source Pages : GitHub Actions.
