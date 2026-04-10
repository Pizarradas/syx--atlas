# Constitution — Sistema de modos SYX

Este documento es la constitución del sistema de modos. Define qué hace cada modo, dónde terminan sus competencias, cómo se componen y qué ocurre cuando hay conflicto.

> **Contexto fundamental:** Este sistema de modos opera dentro del contexto de Atlas. Cuando el prefijo `[ATLAS]:` está activo, Atlas decide qué construir y por qué — los modos SYX ejecutan esa decisión. Las reglas Atlas tienen precedencia sobre las preferencias de modo en decisiones editoriales, de jerarquía y de nomenclatura del proyecto. Ver `mind-system/governance/README.md` para las reglas completas de compatibilidad Atlas+SYX.

---

## Mapa de dominios

Cada modo SYX tiene un dominio exclusivo. Operar fuera del dominio propio es un error de composición.

| Modo | Dominio | Naturaleza | Produce |
|------|---------|------------|---------|
| **SKETCH** | Exploración visual rápida | Generativo | HTML + inline CSS (no producción) |
| **UX** | Estructura, flujo y accesibilidad | Generativo + evaluativo | HTML semántico, component decisions, estado de interacción |
| **CREATIVE** | Experimentación técnica avanzada | Generativo | HTML + CSS experimental, auto-contenido |
| **TOKEN** | Arquitectura del sistema de tokens | Operativo | Archivos de token, `tokens.json`, mapeos semánticos |
| **THEME** | Configuración de temas visuales | Operativo | `_theme.scss`, escalas OKLCH, cobertura de tokens de superficie |
| **UI** | Implementación SCSS de componentes | Operativo | SCSS conforme R01–R04, archivos de token, registro de componente |
| **AUDIT** | Revisión de conformidad | Evaluativo | Informe de violaciones R01–R08, veredicto |
| **MIGRATE** | Resolución de deuda técnica | Operativo | Plan de migración variable a variable |

---

## Jerarquía de tiers

Los modos están ordenados por coste de contexto IA y complejidad de output:

```
Tier 1 — SKETCH     → exploración, sin lectura de archivos
Tier 2 — UX         → estructura y accesibilidad
Tier 3 — CREATIVE   → experimentación, exento de contratos
Tier 4 — TOKEN      → capa de tokens
Tier 5 — THEME      → configuración de paleta
Tier 6 — UI         → implementación SCSS
Tier 7 — AUDIT      → verificación de conformidad
Tier 8 — MIGRATE    → resolución de deuda legacy
```

Regla: usar el tier más bajo que cumpla el objetivo. No escalar innecesariamente.

---

## Zonas de solapamiento y cómo se resuelven

### UX vs UI
- **UX** decide *qué* construir: componentes, HTML, accesibilidad, estados.
- **UI** decide *cómo* implementarlo: SCSS, tokens, layers.
- UX nunca escribe SCSS. UI nunca toma decisiones UX.
- Si UI descubre un problema de accesibilidad durante la implementación → escala a UX para rediseño, no lo resuelve en SCSS.

### TOKEN vs UI
- **TOKEN** crea el sistema de tokens. Solo trabaja en `scss/abstracts/tokens/` y `tokens.json`.
- **UI** consume los tokens definidos por TOKEN. No crea tokens en los archivos de componente.
- Si UI necesita un token que no existe → invoca TOKEN primero, luego vuelve a UI.

### THEME vs TOKEN
- **TOKEN** define los valores semánticos por defecto (tier 2 semántico).
- **THEME** sobreescribe primitivos por tema y los mapea a semánticos. No toca los semánticos por defecto.
- Si THEME necesita un token semántico nuevo que no existe en el sistema → TOKEN lo crea primero.

### AUDIT vs MIGRATE
- **AUDIT** identifica y reporta violaciones. No modifica código.
- **MIGRATE** ejecuta la resolución de una variable a la vez, con plan de impacto previo.
- El flujo correcto es siempre: AUDIT → genera lista → MIGRATE → ejecuta ítem por ítem.

### CREATIVE vs UI
- **CREATIVE** produce prototipos experimentales exentos de contratos R01–R08.
- **UI** es la vía para llevar cualquier output de CREATIVE a producción.
- CREATIVE nunca produce código que se lleve directamente a `scss/` de producción.

---

## Sintaxis de invocación

### Modo único
```
[SYX: SKETCH]: descripción del trabajo
[SYX: UX]: descripción del trabajo
[SYX: UI]: descripción del trabajo
[SYX: TOKEN]: descripción del trabajo
[SYX: THEME]: descripción del trabajo
[SYX: AUDIT]: descripción del trabajo
[SYX: MIGRATE]: descripción del trabajo
[SYX: CREATIVE]: descripción del trabajo
```

### Pipeline secuencial (`→`)
El output de un modo es el input del siguiente. Se ejecutan en orden.

```
[SYX: UX → TOKEN → UI]: diseña, tokeniza e implementa un campo de búsqueda con autocompletar
→ 1. UX define HTML y requisitos de accesibilidad
→ 2. TOKEN define los tokens necesarios
→ 3. UI implementa el SCSS conforme
```

### Composición evaluativa (`+`)
Ambos modos operan sobre el mismo artefacto. Sus outputs se presentan juntos.

```
[SYX: UI + AUDIT]: implementa y valida el componente atom-badge
→ 1. UI implementa
→ 2. AUDIT revisa el output de UI antes de darlo por completado
```

**Precedencia**: `→` tiene mayor precedencia que `+`. La forma `[SYX: UX → UI + AUDIT]:` se lee como `UX → (UI + AUDIT)`. Ver `index.md` para la tabla completa de combinaciones válidas e inválidas.

---

## Protocolos de ejecución combinada

### Protocolo estándar de componente nuevo
```
1. SKETCH    → confirmar que la idea funciona (opcional, bajo coste)
2. UX        → HTML semántico, componentes, accesibilidad, estados
3. TOKEN     → tokens que el componente necesita
4. UI        → implementación SCSS
5. AUDIT     → verificación final
```

En una sola invocación: `[SYX: SKETCH → UX → TOKEN → UI + AUDIT]:`

### Protocolo de tema nuevo
```
1. TOKEN     → verificar que los semánticos de base cubren el tema
2. THEME     → escala OKLCH, _theme.scss, cobertura de 12 tokens de superficie
3. AUDIT     → verificar el _theme.scss contra el contrato de tema
```

En una sola invocación: `[SYX: TOKEN → THEME + AUDIT]:`

### Protocolo de migración
```
1. AUDIT     → informe completo de R07 (legacy vars)
2. MIGRATE   → plan de migración ordenado por riesgo
3. MIGRATE   → ejecutar variable a variable
4. AUDIT     → verificación final tras cada batch
```

MIGRATE requiere confirmación explícita por variable cuando el riesgo es elevado. No usar en pipeline automático si hay muchas variables.

---

## Jerarquía de knowledge

El sistema tiene dos capas de conocimiento:

### Conceptual (knowledges/)
Fundamentos que razonan los *porqués*: teoría del color, leyes de UX, sistemas tipográficos, accesibilidad WCAG.

Se carga cuando:
- El modo necesita razonar en ambigüedad (decisión no cubierta por una regla explícita)
- El output requiere justificación metodológica
- Hay conflicto entre principios que necesita arbitraje

### Operativo (agents/)
Reglas y procedimientos ejecutables: qué produce cada modo, en qué formato, con qué constraints.

Se carga siempre. Es la capa de ejecución.

**Prioridad**: en caso de conflicto entre knowledge conceptual y regla operativa → la regla operativa prevalece. El knowledge conceptual informa las reglas; no las sobreescribe en tiempo de ejecución.

---

## Reglas de prioridad (conflict resolution)

1. **Los contratos R01–R08 son no-negociables** en UI, AUDIT y MIGRATE. Ninguna justificación conceptual los anula.
2. **WCAG AA es el suelo de accesibilidad**. Ninguna decisión estética la justifica.
3. **SKETCH y CREATIVE están exentos de contratos**. Sus outputs nunca van a producción directamente.
4. **TOKEN es upstream de UI y THEME**. Si TOKEN no ha definido un token, UI no puede usarlo.
5. **AUDIT tiene la última palabra en conformidad**. Un output de UI que no pasa AUDIT no está terminado.
6. **Los modos operativos (TOKEN, UI, THEME, MIGRATE) se ejecutan de forma secuencial** cuando se combinan en un pipeline `→`. La composición evaluativa `+` permite que dos modos trabajen sobre el mismo artefacto en paralelo — esto es válido cuando uno genera y el otro evalúa (ej: `UI + AUDIT`), pero no cuando ambos son operativos sobre distintos artefactos.

---

## Compatibilidad con Atlas

Este sistema de modos opera dentro del contexto de Atlas. Las reglas Atlas tienen precedencia sobre las preferencias de modo en todo lo que concierne a:

- Convenciones editoriales y de nomenclatura del proyecto
- Decisiones de arquitectura de carpetas
- Estilos de escritura y nomenclatura de tokens específicos del proyecto

Cuando hay conflicto entre una regla de modo SYX y una regla Atlas → Atlas prevalece.

La carpeta `governance/` en la raíz de `mind-system/` define las reglas de compatibilidad Atlas+SYX a nivel de sistema. Punto de entrada: `governance/README.md`.
