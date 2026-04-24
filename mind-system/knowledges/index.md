# Knowledges — Mapa de dominios

Base de conocimiento conceptual del sistema SYX. Cada módulo fundamenta las reglas operativas de los modos — explica el *porqué* detrás de las decisiones de diseño e implementación.

---

## Estructura

```
knowledges/
  ux/        → Teoría y práctica de UX: leyes cognitivas, heurísticas, escritura
  ui/        → Diseño visual: color, tipografía, movimiento, composición
  front/     → Implementación front: semántica HTML, CSS architecture, a11y, mobile-first
  syx/       → Sistema SYX específico: tokens, SCSS pipeline, componentes, temas, color
  branding/  → Percepción de marca: prestigio, credibilidad, semiótica visual, autoridad
```

---

## Índice por dominio

### `ux/` — UX y diseño de interacción

| Módulo | Contenido | Cargado por |
|--------|-----------|-------------|
| `index.md` | Mapa del dominio UX | — |
| `laws-of-ux.md` | Fitts, Hick, Miller, Jakob, Tesler, Doherty | UX mode |
| `nielsen-heuristics.md` | 10 heurísticas de Nielsen — aplicación práctica | UX mode |
| `dont-make-me-think.md` | Carga cognitiva y diseño auto-evidente (Krug) | UX mode |
| `strategic-writing-for-ux.md` | Microcopy, CTAs, mensajes de error, labels | UX mode |
| `microinteractions.md` | Diseño de estados y feedback de interacción | UX mode |

### `ui/` — Diseño visual

| Módulo | Contenido | Cargado por |
|--------|-----------|-------------|
| `index.md` | Mapa del dominio UI | — |
| `refactoring-ui.md` | Spacing, jerarquía visual, composición (Wathan & Schoger) | UI mode |
| `color-theory.md` | Distribución 60/30/10, semántica del color | TOKEN, THEME |
| `typography-systems.md` | Escalas modulares, line-height, letter-spacing, clamp() | UI mode |
| `motion-principles.md` | Easing, duración, GPU-composited properties | UI, CREATIVE |
| `practical-ui.md` | Correcciones ópticas, densidad, elevación | CREATIVE, UI |

### `front/` — Front-end implementation

| Módulo | Contenido | Cargado por |
|--------|-----------|-------------|
| `index.md` | Mapa del dominio front | — |
| `html-semantics.md` | Elementos semánticos, jerarquía de headings, ARIA base | UX mode |
| `css-architecture.md` | Cascada, especificidad, @layer, progressive enhancement CSS | UI mode |
| `mobile-first.md` | Worst-case first, breakpoints min-width, DOM order | UX, UI, SKETCH, AUDIT |
| `progressive-enhancement.md` | Capas HTML/CSS/JS independientes | UX, AUDIT |
| `accessibility-wcag.md` | WCAG 2.1/2.2 AA, ARIA, gestión del foco | UX, AUDIT |
| `javascript-patterns.md` | Patrones JS para interactividad accesible | UX mode |

### `syx/` — Sistema SYX

| Módulo | Contenido | Cargado por |
|--------|-----------|-------------|
| `index.md` | Mapa del dominio SYX | — |
| `token-system.md` | Cuatro tiers, contratos, naming convention | TOKEN, UI, AUDIT, MIGRATE |
| `scss-pipeline.md` | @mixin template, @layer, mixins de referencia | UI, AUDIT |
| `component-patterns.md` | BEM, prefijos atom/mol/org, estructura de componente | UI, AUDIT, SKETCH, MIGRATE |
| `theme-system.md` | Estructura de `_theme.scss`, secciones obligatorias | THEME, AUDIT |
| `color-oklch.md` | Por qué OKLCH, construcción de escalas, dark mode | TOKEN, THEME |

### `branding/` — Percepción de marca

| Módulo | Contenido | Cargado por |
|--------|-----------|-------------|
| `index.md` | Mapa del dominio branding | — |
| `perception-of-prestige-foundations.md` | Psicología cognitiva del prestigio: modelo PRI, processing fluency, heurísticos de Cialdini, semiótica del lujo | CREATIVE, UX (on-demand) |
| `perception-of-prestige.rules.md` | 17 reglas operativas con checks y output template para auditoría de percepción | CREATIVE (on-demand), AUDIT (on-demand) |

### `vendors/` — Bibliotecas de referencia externas

Conocimiento de fuentes externas integrado en el sistema. **No se carga automáticamente** — se consulta on-demand cuando se solicita una estética, referente o técnica concreta.

| Biblioteca | Contenido | Cargado por |
|-----------|-----------|-------------|
| `vendors/awesome-design/index.md` | Catálogo de 58 DESIGN.md de empresas reales (Vercel, Stripe, Linear, Apple…) organizados por estética, tipografía y filosofía de sombras/bordes | CREATIVE, SKETCH, THEME (on-demand) |
| `vendors/awesome-design/DESIGN.md` | DESIGN.md completo de Vercel — referencia de system design minimalista engineering | CREATIVE, THEME (on-demand) |
| `vendors/awesome-design/awesome-design-md-main/design-md/[empresa]/DESIGN.md` | DESIGN.md individual de cada empresa | On-demand por nombre |

---

## Cómo se usa este knowledge

1. Los modos cargan módulos específicos según su dominio (ver tabla en `agents/index.md`).
2. Cuando una regla operativa no cubre un caso → consultar el módulo de knowledge del dominio.
3. El knowledge conceptual informa; las reglas operativas ejecutan. En caso de conflicto, las reglas operativas prevalecen.
4. Para añadir nuevo conocimiento: crear el archivo en el dominio correspondiente con el schema `## meta / ## concepts / ## rules / ## checklist`.
