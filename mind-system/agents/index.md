# Index — Sistema de modos SYX

Referencia rápida de los 8 modos del sistema. Para la constitución completa ver `constitution.md`.

---

## Tabla de modos

| Modo | Activación | Rol | Fidelidad | Usa SYX DS | Tier |
|------|-----------|-----|-----------|------------|------|
| **SKETCH** | `[SYX: SKETCH]:` | Rapid prototyper | Boceto | Nomenclatura BEM únicamente | 1 |
| **UX** | `[SYX: UX]:` | UX consultant | Media-Alta | Selección de componentes | 2 |
| **CREATIVE** | `[SYX: CREATIVE]:` | Creative director | Experimental | Opcional / guiado | 3 |
| **TOKEN** | `[SYX: TOKEN]:` | Token architect | Operativo | Solo capa de tokens | 4 |
| **THEME** | `[SYX: THEME]:` | Theme designer | Operativo | Solo `_theme.scss` | 5 |
| **UI** | `[SYX: UI]:` | Senior SCSS developer | Alta | Implementación completa | 6 |
| **AUDIT** | `[SYX: AUDIT]:` | QA reviewer | Evaluativo | Lectura completa | 7 |
| **MIGRATE** | `[SYX: MIGRATE]:` | Migration specialist | Operativo | Resolución legacy | 8 |

---

## Knowledge cargado por modo

| Modo | syx/ | ux/ | ui/ | front/ | branding/ | motion/ | vendors/ |
|------|------|-----|-----|--------|-----------|---------|---------|
| SKETCH | nomenclatura BEM | — | — | mobile-first | — | 04-glosario (on-demand: si el brief usa términos de motion) | awesome-design (on-demand: estética en brief) |
| UX | component-patterns | laws-of-ux, nielsen-heuristics, dont-make-me-think | — | mobile-first, accessibility-wcag, progressive-enhancement, javascript-patterns (si JS) | perception-of-prestige-foundations (on-demand: contexto de confianza/autoridad) | — | — |
| CREATIVE | scss-pipeline (ref) | — | color-theory, motion-principles, typography-systems, practical-ui | — | perception-of-prestige-foundations (siempre), perception-of-prestige.rules (on-demand: brief premium/credibilidad) | **01-fundamentos, 02-capacidades, 03-patrones, 04-glosario** (siempre cuando hay GSAP) | awesome-design (on-demand: referente en brief) |
| TOKEN | token-system, color-oklch | — | color-theory | **size-models** (escala), **size-models-checklist** (self-check) | — | — | — |
| THEME | token-system, theme-system, color-oklch | — | color-theory | **size-models** (primitivos de escala) | — | — | awesome-design (on-demand: referente de paleta) |
| UI | scss-pipeline, component-patterns, token-system | — | refactoring-ui, typography-systems, motion-principles | mobile-first, css-architecture, **size-models** (tipografía/espaciado) | — | **03-patrones** (on-demand: si el componente requiere efectos GSAP); **01-fundamentos** (on-demand: para hacer preguntas precisas) | — |
| AUDIT | scss-pipeline, component-patterns, token-system | — | — | accessibility-wcag, mobile-first, **size-models-checklist** (coherencia de escala) | perception-of-prestige.rules (on-demand: auditoría de percepción de marca) | — | — |
| MIGRATE | token-system, component-patterns | — | — | — | — | — | — |

---

## Operadores de combinación

Dos operadores definen cómo se relacionan los modos cuando se combinan:

| Operador | Sintaxis | Semántica |
|----------|----------|-----------|
| `→` | `[SYX: UX → UI]:` | **Pipeline secuencial** — el output de cada modo es el input del siguiente. Se ejecutan en orden, un paso a la vez. |
| `+` | `[SYX: UI + AUDIT]:` | **Evaluativa** — ambos modos operan sobre el mismo input. El resultado de cada uno se presenta en paralelo en la misma respuesta. |

**Precedencia cuando se mezclan**: `→` tiene mayor precedencia que `+`. La forma `[SYX: UX → UI + AUDIT]:` se lee como `UX → (UI + AUDIT)`: UX primero, luego UI implementa y AUDIT verifica ese mismo output.

---

## Combinaciones válidas

### Pipeline secuencial `→`

El output de cada modo es el input del siguiente. Cada paso hace handoff explícito.
Si un paso intermedio concluye que no hay trabajo (ej: TOKEN determina que los tokens necesarios ya existen), el pipeline continúa con los existentes y lo indica.

| Sintaxis | Cuándo usar |
|----------|-------------|
| `[SYX: SKETCH → UX]:` | Validar una idea antes de formalizar la accesibilidad |
| `[SYX: UX → UI]:` | Componente nuevo sin necesidad de tokens propios |
| `[SYX: UX → TOKEN → UI]:` | Flujo estándar de componente nuevo |
| `[SYX: UX → TOKEN → UI → AUDIT]:` | Flujo completo con verificación |
| `[SYX: TOKEN → THEME]:` | Crear un tema nuevo después de verificar cobertura de semánticos |
| `[SYX: AUDIT → MIGRATE]:` | Resolver deuda técnica identificada |
| `[SYX: CREATIVE → TOKEN → UI]:` | Llevar un experimento creativo a producción |
| `[SYX: CREATIVE → TOKEN → UI → AUDIT]:` | Experimento a producción con verificación |

### Evaluativa `+`

Ambos modos trabajan sobre el mismo artefacto. Sus outputs se presentan juntos en la misma respuesta.

| Sintaxis | Cuándo usar |
|----------|-------------|
| `[SYX: UI + AUDIT]:` | Implementar y verificar en el mismo flujo |
| `[SYX: THEME + AUDIT]:` | Crear un tema y verificar cobertura inmediatamente |
| `[SYX: UX + AUDIT]:` | Propuesta HTML con verificación de jerarquía y ARIA |
| `[SYX: TOKEN + AUDIT]:` | Definir tokens y verificar contratos R05–R08 |

### Combinaciones no válidas

| Combinación | Por qué no |
|-------------|------------|
| `SKETCH + AUDIT` | SKETCH está exento de contratos por diseño. Auditarlo es contradictorio. |
| `SKETCH + UI` | SKETCH es boceto, UI es producción. No hay handoff útil en el mismo turno. |
| `CREATIVE + AUDIT` | CREATIVE está exento de R01–R08. Usar `CREATIVE → TOKEN → UI → AUDIT` si se quiere verificar. |
| `UI → TOKEN` | El orden correcto es TOKEN primero. TOKEN define, UI implementa. |
| `THEME → UI` | THEME opera en `_theme.scss`. No genera componentes para que UI procese. |
| `MIGRATE + AUDIT` | AUDIT detecta, MIGRATE resuelve. Solo tiene sentido como `AUDIT → MIGRATE`. |

---

## Recursos auxiliares

### Prompts (`../prompts/`)
Plantillas de invocación para tareas frecuentes. No son modos — son input estructurado para modos.

| Archivo | Para qué modo | Cuándo usar |
|---------|--------------|-------------|
| `new-atom.md` | UI | Crear un átomo nuevo desde cero |
| `new-molecule.md` | UI | Crear una molécula nueva desde cero |
| `review-component.md` | AUDIT | Auditar SCSS existente con checklist completo |
| `theme-audit.md` | AUDIT | Auditar `_theme.scss` con los 12 tokens obligatorios |

### Workflows (`../workflows/`)
Procedimientos paso a paso para operaciones de mayor alcance. Complementan los modos.

| Archivo | Modos implicados | Cuándo usar |
|---------|-----------------|-------------|
| `audit-tokens.md` | AUDIT | Auditoría completa del sistema de tokens (7 checks) |
| `create-component.md` | TOKEN + UI | Crear componente nuevo con registro completo |
| `create-theme.md` | TOKEN + THEME | Crear tema nuevo con 8 pasos + checklist |
| `update-changelog.md` | — | Actualizar CHANGELOG antes de publicar versión |

---

## Boundaries de modo

Lo que cada modo nunca hace:

| Modo | Nunca hace |
|------|-----------|
| SKETCH | Produce código de producción; lee `tokens.json`; valida contratos |
| UX | Escribe SCSS; nombra tokens; toma decisiones de implementación |
| CREATIVE | Garantiza conformidad R01–R08; produce código listo para `scss/` |
| TOKEN | Toca SCSS de componente; configura temas |
| THEME | Toca SCSS de componente; define semánticos por defecto |
| UI | Toma decisiones UX; crea tokens sin TOKEN previo |
| AUDIT | Modifica código; sugiere nuevas features; valora "estilo" |
| MIGRATE | Migra más de una variable por turno; toca variables `keep` |

---

## Flujos típicos de referencia

### Componente nuevo (flujo completo)
```
[SYX: SKETCH → UX → TOKEN → UI + AUDIT]:
```
Si se necesita confirmación antes de implementar, separar en dos turnos:
```
Turno 1: [SYX: SKETCH → UX]:
Turno 2: (confirmado) [SYX: TOKEN → UI + AUDIT]:
```

### Componente nuevo sin tokens propios
```
[SYX: UX → UI + AUDIT]:
```

### Validación rápida de idea
```
[SYX: SKETCH → UX]:
```

### Revisión de deuda técnica
```
Turno 1: [SYX: AUDIT]:     → informe completo, ordenar por riesgo
Turno 2: [SYX: MIGRATE]:   → variable por variable (un turno por variable)
```
No usar `AUDIT → MIGRATE` en pipeline automático si el número de variables es alto.
MIGRATE requiere confirmación explícita por variable cuando el riesgo es elevado.

### Exploración creativa → producción
```
Turno 1: [SYX: CREATIVE]:                    → prototipo experimental
Turno 2: [SYX: TOKEN → UI + AUDIT]:          → llevar a producción con verificación
```
