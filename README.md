# Aero-design-system

Design System production-ready pour industrie aéronautique & métrologie.
Tokens-first, composants accessibles (WCAG AAA), démo interactive.

{WIP}
Ce repository partage un projet en cours de réalisation, afin de constituer une carte des processus de construction d'un modèle de design system modulable pour l'IA.
---

## Pourquoi ce DS?

Les interfaces critiques en aéronautique exigent **précision**, **fiabilité**, **accessibilité**. Ce système garantit:

(en cible)
- ✅ **Cohérence visuelle** : tokens centralisés, appliqués partout
- ✅ **Accessibilité vraie** : WCAG AA (4.5:1 contraste), keyboard nav, screen reader ready
- ✅ **Scalabilité** : 25 composants core, extensible
- ✅ **Maintenabilité** : source unique (JSON) → multi-output (SASS, CSS, JSON)
- ✅ **Performance** : CSS minimal, pas de runtime dépendances

---

## Quick Start

```bash
git clone https://github.com/ton-username/aerospace-design-system.git
cd aero-design-system

npm install
npm run dev
```

---

## Architecture

```
tokens/              # Design tokens (source unique en JSON)
  ├─ design-tokens.json
  └─ themes/         # light.json, dark.json

.style-dictionary/   # Token transformer
  ├─ config.js
  ├─ transforms/
  └─ formats/

src/
  ├─ styles/
  │  ├─ generated/   # Output style-dictionary (auto)
  │  ├─ base/        # Reset, typo, utilities
  │  └─ index.scss   # Import tout
  └─ components/     # 25 composants HTML/SCSS
    ├─ button/
    ├─ input/
    ├─ table/
    ├─ modal/
    ├─ ... (23 autres)

demo/                # Dashboard aéronautique (démo live)

docs/                # Guides (tokens, components, a11y)

figma/               # Design system Figma (variables + components)
```

---

## Tokens → Code 

### 1️⃣ Éditer les tokens
```json
// tokens/design-tokens.json
{
  "color": {
    "primary": {
      "500": { "value": "#2563eb" }
    }
  },
  "spacing": {
    "4": { "value": "16px" }
  }
}
```

### 2️⃣ Build les tokens
```bash
npm run tokens:build
```

### 3️⃣ Utilise dans SCSS
```scss
// src/components/button/button.scss
@import '../../styles/generated/variables';

.btn {
  background-color: $color-primary-500;
  padding: $spacing-4;
}
```

### Résultat
Une **source unique** générant:
- `_variables.scss` (SASS variables)
- `variables.css` (CSS custom properties)
- `tokens-meta.json` (auto-documentation)

---

## Les 25 Composants (v1.0)

| | | |
|---|---|---|
| **Inputs** | Button | Input, Textarea |
| | Select | Checkbox, Radio, Toggle |
| **Display** | Table | Card, Badge |
| | Alert | Notification |
| **Navigation** | Breadcrumb | Tabs, Accordion |
| | Pagination | Stepper, Menu |
| **Feedback** | Spinner | Progress |
| **Other** | Modal | Dropdown, Tooltip |
| | Avatar | Chip, Link, Divider |

Chaque composant:
- 2-4 variantes (size, state, variant)
- Keyboard accessible (Tab, Enter, Escape, arrows)
- ARIA labels & roles
- Tested on screen readers

---

## Accessibilité (WCAG AAA)

✅ **Contraste**
- Texte/fond: 4.5:1 > (AAA)
- Graphiques: 3:1 > (AAA)

✅ **Keyboard Navigation**
- Tab: forward through interactive elements
- Shift+Tab: backward
- Enter: activate button/link
- Escape: close modal/popover
- Arrows: navigate menus/tabs

✅ **Screen Reader**
- ARIA labels descriptives
- Semantic HTML (`<button>`, `<nav>`, etc.)
- Live regions pour notifications
- Hidden decorative elements (`aria-hidden="true"`)

✅ **Focus Management**
- Focus visible (2px outline)
- Logical tab order
- Focus trap dans modals

✅ **Mobile & Touch**
- Touch targets ≥ 48x48px
- No hover-only controls

Détails complets: [docs/ACCESSIBILITY.md](docs/ACCESSIBILITY.md)

---

## Démo Interactive

Dashboard aéronautique fonctionnel utilisant TOUS les composants:

- 📊 Tableaux temps réel (production line status)
- 🔔 Notifications (alerte température, défaut capteur)
- 📈 Monitoring (KPIs usine)
- 🎛️ Contrôles (filtres, recherche, pagination)

→ Démontre les composants en contexte réel, pas juste isolés.

**Deploy** : Vercel (auto-deploy sur `main`)

---

## Scripts NPM

```bash
# Développement
npm run dev              # Watch tokens + SASS

# Build
npm run tokens:build     # Style-dictionary → SASS/CSS
npm run sass:build       # Compile SCSS
npm run build            # Tokens + SASS (ci)

# Tests
npm run a11y:test        # Axe accessibility tests
npm run lighthouse       # Lighthouse audit

# Serve
npm run serve            # Ouvre sur http://localhost:8000
```

---

## Workflow Integration

### Figma ↔ Code (sync tokens)

1. **Design tokens en Figma** (variables de composants)
2. **Export JSON** via Figma Tokens plugin
3. **Copie dans** `tokens/design-tokens.json`
4. **Build** → SCSS/CSS auto-généré
5. **Composants** utilisent les tokens générés

### Git Workflow

```bash
# Feature: nouveau composant
git checkout -b feat/chip-component

# Work
npm run dev
# ... code chip.scss, chip.html

# Commit
git add src/components/chip/
git commit -m "feat(chip): add chip component with variants"

# PR → review → merge
```

---

## Utilisation dans un Projet

### Option 1: Copy-paste (pour MVP)
Copie les fichiers HTML/SCSS du composant que tu besoin.

### Option 2: Import SASS (pour projet SASS)
```scss
@import 'path/to/tokens/generated/variables';
@import 'path/to/components/button/button';
```

### Option 3: Publish NPM (v2.0)
```bash
npm install @aerospace/design-system
```
(Prévu en v2.0)

---

## Thèmes (Light / Dark)

Définis dans `tokens/themes/`:

```bash
npm run tokens:build --filter=light
npm run tokens:build --filter=dark
```

CSS custom properties auto-générées:
```css
:root {
  --color-primary-500: #2563eb;
}

@media (prefers-color-scheme: dark) {
  :root {
    --color-primary-500: #1e40af;
  }
}
```

---

## Versioning & Changelog

Suit [Semantic Versioning](https://semver.org/):

- **v1.0.0** : Release initial (25 components, tokens-first)
- **v1.1.0** : Thèmes étendus, animations, `@supports`
- **v2.0.0** : Web components, React wrapper, NPM package

Voir [CHANGELOG.md](docs/CHANGELOG.md) pour détails.

---

## Documentation Complète

| Guide | Contenu |
|-------|---------|
| **[Design Tokens](docs/DESIGN_TOKENS.md)** | Taxonomy, naming, usage tokens |
| **[Components](docs/COMPONENTS.md)** | Chaque composant: props, HTML, variantes |
| **[Accessibility](docs/ACCESSIBILITY.md)** | WCAG AAA, RGAA 4.1, tests |
| **[Contributing](docs/CONTRIBUTING.md)** | Comment ajouter un composant |
| **[Changelog](docs/CHANGELOG.md)** | Versions & breaking changes |
| **[Roadmap](docs/ROADMAP.md)** | v1.1, v2.0, vision future |

---

## Figma Setup

Le design system Figma est synchronisé avec le code:

1. **Variables Figma** = tokens JSON
2. **Components Figma** = variantes des composants
3. **Documentation** = component descriptions

Accès: [Figma Aerospace DS](https://figma.com/...)

---

## Performance

- **CSS**: 12 KB minifié (tokens + composants)
- **HTML** : semantic, zéro bloat
- **JS**: 0 KB (pur CSS/HTML, pas de runtime)
- **Lighthouse** : 95+ (perf, a11y, best practices)

---

## Contribution

Veux ajouter un composant ou améliorer l'accessibilité?

1. Fork le repo
2. Crée une branch: `feat/mon-composant`
3. Suit [CONTRIBUTING.md](docs/CONTRIBUTING.md)
4. Push & PR

---

## License

MIT - Libre d'utilisation

---

## Crédits

Designé pour industrie aéronautique & métrologie.
Stack: Figma | Style-Dictionary | SASS | HTML5 | WCAG AAA

---

**Questions?** Ouvre une issue ou contacte [ton-email].
