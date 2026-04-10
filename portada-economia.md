# Meridian Economía — Portada de referencia

Prototipo de portada para un medio especializado en economía, construido sobre el principio de que **la publicidad es el mapa a partir del cual se lee la información**.

El archivo `portada-economia.html` es completamente autocontenido: HTML, CSS y JS en un único fichero. Ábrelo en cualquier navegador para explorarlo.

---

## El axioma fundacional

En la mayoría de sistemas editoriales, el grid se diseña primero y la publicidad se encaja después. El resultado habitual es una experiencia rota.

Este prototipo invierte esa lógica:

> Los formatos IAB no son restricciones. Son las constantes espaciales del sistema. Todo lo demás deriva de ellos.

---

## Modelo matemático

### Formatos IAB → constantes del sistema

| Formato | Dimensión | Rol en el sistema |
|---|---|---|
| NP-1 Site Skin (×2) | 220px de ancho | Frame lateral — define el canvas total a ≥1440px |
| NP-2 Leaderboard | 728 × 90px | Zona de cabecera — ancla el ancho del nav |
| NP-3 Medium Rectangle | 300 × 250px | Columna sidebar — exacto, sin redondeo |
| NP-4 Billboard | 970 × 90px | Pausa estructural entre zonas editoriales |

### Derivaciones del grid

```
Canvas total @ ≥1440px  =  220 + 1200 + 220  =  1640px
Contenedor max          =  1200px  (centrado)
Sidebar                 =   300px  =  NP-3.width         [EXACTO — sin redondeo]
Gap editorial           =    32px  =  8 × 4px
Columna principal       =   868px  =  1200 − 300 − 32
Ratio main : sidebar    =  2.89:1  ≈  3:1                [editorial clásico]
```

### Escala tipográfica — Major Third ×1.25 sobre base 16px

```
label     0.6875rem  =  11px   + 0.08em tracking
meta      0.75rem    =  12px
body-sm   0.875rem   =  14px
body      0.9375rem  =  15px
h3        1.125rem   =  18px
h2        clamp(1.375rem, 2vw + 0.375rem, 1.875rem)   →  22–30px
display   clamp(2rem, 3.5vw + 0.75rem, 3.375rem)      →  32–54px
```

### Ritmo vertical — múltiplos de 8px

```
Ticker strip       40px   (5 × 8)
Leaderboard zone  106px   (90px NP-2 + 2 × 8px padding)
Header nav         72px   (9 × 8)
Billboard zone    106px   (90px NP-4 + 2 × 8px padding)
```

### Color — regla 60/30/10

```
60%  Superficies   #f8f7f4 (papel)  ·  #ffffff (tarjetas)
30%  Tinta         #1a1a1a  ·  #374151  ·  #6b7280  ·  #e2e0db (bordes)
10%  Acento        #bf2025  — solo elementos interactivos o de jerarquía clave
```

---

## Densidad publicitaria

Según ATLAS 06.3, el máximo es **3 bloques NP por portada**.

| Zona | Bloques NP | Formato |
|---|---|---|
| Hero N1 | 0 | — |
| Desarrollo | 1 | NP-3 en sidebar |
| Profundidad | 1 | NP-4 billboard entre zonas |
| Cabecera | 1 | NP-2 leaderboard |
| **Total** | **3** | **Límite exacto** |

---

## Capas del sistema

Este prototipo es el output de tres capas diferenciadas:

### ATLAS
Sistema de reglas editoriales. Define jerarquía de contenido (N1/N2/N3), zonas de lectura (entrada, desarrollo, profundidad), densidad máxima, y posiciones permitidas y prohibidas para cada formato publicitario.

### SYX
Sistema de diseño token-driven (v4.1.0). Contratos ejecutables: ningún valor de color fuera del registro, ningún px hardcodeado, ninguna rotura del grid. La capa técnica que hace que el sistema sea reproducible.

### Modos de IA
Cada tipo de tarea tiene su modo, su coste de recursos y sus artefactos esperados. No es lo mismo generar un wireframe (SKETCH) que auditar contratos (AUDIT) o migrar tokens (MIGRATE). La IA no diseña — ejecuta un sistema que alguien definió antes.

---

## Cómo explorarlo

1. Abre `portada-economia.html` en un navegador moderno
2. Para ver el Site Skin (NP-1), amplía la ventana a ≥1440px
3. El menú hamburguesa y el buscador son funcionales
4. Redimensiona para ver el comportamiento responsive
5. Inspecciona el CSS: todas las constantes están documentadas en el bloque de comentario inicial de `<style>`

---

## Tipografías

- **Playfair Display** (700, 800) — titulares editoriales
- **Inter** (400, 500, 600) — interfaz y cuerpo de texto

Servidas desde Google Fonts. Sin conexión, el sistema cae sobre Georgia y `-apple-system`.

---

## Contexto

Este prototipo es parte de una investigación sobre sistemas editoriales donde la lógica de negocio (publicidad) y la experiencia de lectura no compiten, sino que se diseñan como un sistema coherente desde el origen.

La distinción que importa no es *usar IA* sino *definir qué puede y qué no puede hacer*.
