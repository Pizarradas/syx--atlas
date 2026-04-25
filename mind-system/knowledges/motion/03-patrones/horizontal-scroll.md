# Horizontal scroll

## Intención
Convertir el scroll vertical en un recorrido lateral — usado para timelines, carruseles narrativos, "panels" de storytelling.

## Definición
Una sección pinneada cuya pista interna (`__track`) tiene `display: flex` y desplaza su `x` en negativo proporcional al progreso del scroll. La distancia de scroll vertical equivale al ancho excedente de la pista (`scrollWidth - innerWidth`).

## Sinónimos útiles
Sideways panels, horizontal storytelling, lateral scroll section, panel scrubbing.

## Se suele confundir con
- **Carousel infinito**: aquí no hay loop; es lineal.
- **Snap scroll horizontal**: la mayoría de implementaciones del horizontal-scroll-pinned son scrub continuo, no snap.

## Control GSAP
```js
const track = section.querySelector(".track");
const distance = () => track.scrollWidth - window.innerWidth + 80;

gsap.to(track, {
  x: () => -distance(),
  ease: "none",
  scrollTrigger: {
    trigger: section,
    start: "top top",
    end: () => "+=" + distance(),
    pin: true,
    scrub: 0.8,
    invalidateOnRefresh: true,
    anticipatePin: 1
  }
});
```

## Parámetros sensibles
| Parámetro | Rango / nota |
|---|---|
| `end: "+="` | igual al ancho excedente — esto vincula 1px scroll = 1px desplazamiento horizontal |
| `scrub` | `0.6 — 1.0` (un poco más alto que pinned-scrub vertical, da más sensación de "deriva") |
| `pin-spacing` | Por defecto activado; útil para que el resto del contenido recupere su posición |
| Mobile fallback | Activar `flex-direction: column` y desactivar pin — vertical "natural" |

## Casos de uso
- Cronología (timeline) horizontal — perfecto para vidas, evoluciones
- Galería de "panels" narrativos
- Comparativa visual (antes/después/etapa)
- Catálogo de proyectos en presentación corporativa

## Prompt IA
> "Horizontal scroll pinneado: track flex con N items, distance = trackWidth - viewportWidth, x: -distance al scrollear, pin true, scrub 0.8, end: +=distance. Mobile: flex-direction column sin pin (fallback vertical). Reduced-motion: igual que mobile."

## Fallback mobile y reduced-motion
- `media (max-width: 1023px)`: cambiar `flex-direction` a `column`, padding vertical, sin pin
- `prefers-reduced-motion: reduce`: ignorar el ScrollTrigger y aplicar el mismo CSS de mobile

## Anti-patrones
- **No usar para más de ~8 items**. El usuario pierde la pista del avance.
- **No mezclar con parallax interno**. Demasiado movimiento simultáneo marea.
- **Indicar progreso siempre** (puntos, números, porcentaje) — sin él el usuario no sabe cuánto le queda.
