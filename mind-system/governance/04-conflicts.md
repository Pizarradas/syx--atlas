# 04 — Resolución de conflictos

Cuando una regla de Modes y una regla de Atlas parecen contradecirse, aplicar este orden:

```
1. ¿Es un conflicto técnico de implementación SCSS?
   → Modes prevalece.

2. ¿Es un conflicto de decisión editorial (nivel, jerarquía, densidad)?
   → Atlas prevalece.

3. ¿Es un conflicto de accesibilidad?
   → El estándar más estricto prevalece (generalmente UX mode, que cita WCAG).

4. ¿Es un conflicto de naming de tokens?
   → Modes prevalece (ver 03-domains.md, tabla de puente de tokens).

5. ¿Es un conflicto de formato de color?
   → OKLCH para SCSS (Modes). Hex para POC Atlas standalone.
```

Si el conflicto no encaja en ninguno de estos cinco casos → **escalar**. No resolver de forma implícita. Señalar el conflicto y documentarlo en `atlas-rules/11.0-arbitraje-principios.md`.
