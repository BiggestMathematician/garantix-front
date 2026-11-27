---
name: composant-ds
description: Créer ou modifier un composant du design system Garantix (src/components/) avec ses tokens, son CSS Module, sa story Storybook et ses vérifications d'accessibilité. À utiliser dès qu'on ajoute ou change un composant d'interface.
---

# Composant du design system Garantix

## Avant d'écrire
1. Lire `docs/DESIGN_SYSTEM.md` : le composant est-il listé ? Sinon, l'ajouter au tableau d'abord.
2. Choisir l'élément HTML natif qui porte déjà le rôle (`button`, `a`, `input`, `fieldset`…). ARIA seulement si aucun ne convient.

## Fichiers
```
src/components/Nom/
  Nom.tsx          props typées (type exporté NomProps), pas de any
  Nom.module.css   uniquement des var(--…) sémantiques de tokens.css
  Nom.stories.tsx  une story par variante et par état
```

## Règles
- Aucune valeur en dur : ni couleur, ni px, ni rem, ni durée. Un token manque → l'ajouter dans `tokens.css` et `DESIGN_SYSTEM.md`.
- Jamais `--color-decor` pour du texte ou une bordure de champ.
- Cible cliquable ≥ 44 × 44 px ; focus visible avec `--focus-ring`.
- Information jamais portée par la couleur seule.
- Pas de logique métier dans le composant : il reçoit des données déjà calculées par `src/domain/`.

## Stories
- `title: "Design system/Nom"`, `tags: ["autodocs"]`.
- Couvrir : défaut, chaque variante, désactivé, erreur ou vide si applicable, texte long (débordement à 320 px).

## Vérifier avant de rendre la main
- [ ] `npm run lint` et `npm run build` passent.
- [ ] Story affichée, onglet Accessibility de Storybook sans violation.
- [ ] Composant utilisable au clavier seul.
- [ ] Commit : `feat(ds): Nom [EX-0x]`.
