# Design system — Garantix

Direction reprise de Garantix : papier, encre, vermillon. Filets de 1 px, deux familles de caractères, une jauge guilloché comme signature. Sobre, typographique, sans image photographique.

## Tokens
Deux niveaux dans `src/styles/tokens.css` : les **primitives** (valeurs brutes, jamais utilisées dans un composant) et les **sémantiques** (rôle, seules utilisables dans les CSS Modules). Le mode sombre ne redéfinit que les sémantiques.

### Couleurs
| Token sémantique | Clair | Sombre | Usage |
|---|---|---|---|
| `--color-bg` | `#ffffff` | `#121211` | Fond de page |
| `--color-bg-raised` | `#f6f5f1` | `#1c1c1a` | Cartes, panneaux |
| `--color-text` | `#0d0d0c` | `#f2f1ec` | Texte principal |
| `--color-text-muted` | `#5a5a57` | `#a8a7a0` | Texte secondaire, bordures de champ |
| `--color-decor` | `#9b9b96` | `#6b6a65` | Filets et motifs décoratifs **uniquement** |
| `--color-accent` | `#c2321a` | `#ef6a4f` | Liens, focus, actions, statut Expirée |
| `--color-warning` | `#8a6a10` | `#d9b14a` | Statut Bientôt expirée |
| `--color-success` | `#2f6b3a` | `#7cc48a` | Statut Active |
| `--color-line` | encre à 13 % | papier à 16 % | Filets |

### Contrastes vérifiés (WCAG 2.1, RGAA 3.2 et 3.3)
| Couleur | Sur `bg` clair | Sur `bg-raised` clair | Sur `bg` sombre | Sur `bg-raised` sombre |
|---|---|---|---|---|
| text | 19.44 | 17.82 | 16.57 | 15.09 |
| text-muted | 6.92 | 6.34 | 7.77 | 7.07 |
| accent | 5.58 | 5.11 | 6.11 | 5.56 |
| warning | 5.06 | 4.64 | 9.23 | 8.40 |
| success | 6.39 | 5.86 | 9.03 | 8.23 |
| decor | 2.79 ✗ | 2.56 ✗ | 3.46 ✗ | 3.15 ✗ |

Tout texte atteint 4.5:1. `--color-decor` échoue : il est interdit pour du texte et pour les bordures de champ (qui utilisent `--color-text-muted`, ≥ 3:1).

Un statut n'est jamais porté par la couleur seule : toujours un libellé texte (RGAA 3.1).

### Typographie
| Token | Valeur | Usage |
|---|---|---|
| `--font-display` | serif système (`ui-serif, Georgia, serif`) | Titres |
| `--font-text` | mono système (`ui-monospace, "SF Mono", Menlo, monospace`) | Texte, données |
| `--text-sm` · `--text-md` · `--text-lg` · `--text-xl` · `--text-2xl` | `0.875rem` · `1rem` · `1.25rem` · `clamp(1.5rem, 3vw, 2rem)` · `clamp(2rem, 6vw, 3.5rem)` | Échelle |
| `--leading-tight` · `--leading-body` | `1.15` · `1.6` | Interlignage |

Taille de base 1rem (respecte le réglage du navigateur). Polices système : aucun fichier de police à télécharger.

### Espacements, rayons, mouvement
| Token | Valeur |
|---|---|
| `--space-1` à `--space-8` | `0.25rem` `0.5rem` `0.75rem` `1rem` `1.5rem` `2rem` `3rem` `4rem` |
| `--gutter` | `clamp(1rem, 4vw, 4rem)` |
| `--radius-sm` · `--radius-pill` | `2px` · `999px` |
| `--border` | `1px solid var(--color-line)` |
| `--focus-ring` | `2px solid var(--color-accent)`, décalage `3px` |
| `--duration` · `--ease` | `200ms` · `cubic-bezier(0.16, 1, 0.3, 1)` |

`prefers-reduced-motion: reduce` ramène `--duration` à `0ms`.

### Points de rupture
Mobile d'abord. Les grilles utilisent `repeat(auto-fill, minmax(min(100%, 18rem), 1fr))`, sans media query quand c'est possible. Une seule rupture nommée : `48rem` (en-tête et page détail en deux colonnes).

## Composants
| Composant | Rôle | Lot |
|---|---|---|
| `Button` | Action, variantes `primary` / `ghost`, taille de cible ≥ 44 px | 0 |
| `StatusBadge` | Libellé du statut de garantie, couleur + texte | 0 |
| `Rosette` | Jauge guilloché : pétales = années, anneaux allumés = part restante | 2 |
| `AchatCard` | Carte d'un achat dans la liste, lien vers le détail | 1 |
| `EmptyState` | Message de liste vide ou sans résultat, avec action | 1 |
| `FollowToggle` | Bouton « Suivre », `aria-pressed` | 5 |
| `Field` | Libellé, champ, aide et erreur liés | 4 |
| `FilterBar` | Recherche, catégorie, statut, compteur `aria-live` | 6 |

Chaque composant : un dossier `src/components/Nom/` avec `Nom.tsx`, `Nom.module.css`, `Nom.stories.tsx`. Les stories couvrent chaque variante et chaque état (dont erreur, vide, désactivé).
