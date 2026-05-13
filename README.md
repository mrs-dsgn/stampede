# Stampede v6 — Internal Toolset

Kit de herramientas hecho a mano para el equipo de diseño.
Cada tool hace una cosa y la hace bien.

**URL:** maresmares.github.io/stampede
**Stack:** Vanilla HTML + CSS + JS · Single file por tool · GitHub Pages
**Deployment:** GitHub Desktop — sin build process, sin dependencias

---

## Estructura de archivos

```
stampede/
│
├── index.html              ← Hub / landing
├── _template.html          ← Plantilla base para tools nuevas
├── README.md
│
├── tag/
│   └── index.html          ← Tag v6 — file naming tool
│
├── scale/
│   └── index.html          ← Scale v1 — conversor de unidades
│
└── frame/
    ├── index.html          ← Frame hub — selector de campañas
    └── jac4/
        └── index.html      ← Frame · JAC4 — generador de prompts
```

---

## Cómo agregar una tool nueva

1. Duplicar `_template.html`
2. Renombrarlo a `index.html` y moverlo a su carpeta (`./nombre-tool/index.html`)
3. Abrir un chat nuevo y pegar el archivo con el mensaje:
   > "Este es Stampede v6. Quiero construir una nueva tool llamada [Nombre] que hace [descripción]."
4. El contexto visual y técnico ya viene dentro del template — no hay que re-explicar el design system
5. Al terminar: agregar la card activa en `frame/index.html` o `index.html` según corresponda
6. Actualizar este README

---

## Design System

### Tipografía

| Uso | Fuente |
|-----|--------|
| Títulos, labels, monospace, código, breadcrumb | JetBrains Mono |
| Body copy, descripciones, botones secundarios | Plus Jakarta Sans |

### Colores — tokens

| Token | Valor | Uso |
|-------|-------|-----|
| `--purple` | `#34275f` | Acento primario, brand, iconos activos |
| `--purple-mid` | `#4a3880` | Hover states purple |
| `--orange` | `#df6f40` | Acento secundario, eyebrows, toggles ON |
| `--orange-light` | `#e8845a` | Hover states orange |
| `--bg` | `#f2f1ee` | Fondo general (light) |
| `--surface` | `#ffffff` | Cards, header (light) |
| `--surface-2` | `#f7f6f4` | Inputs, badges, DPI presets (light) |
| `--surface-3` | `#eeece8` | Toggles, prefijos, placeholders (light) |
| `--border` | `rgba(0,0,0,0.08)` | Bordes suaves |
| `--border-mid` | `rgba(0,0,0,0.12)` | Bordes con más presencia |
| `--text-primary` | `#111010` | Texto principal |
| `--text-secondary` | `#5a5855` | Texto secundario, labels |
| `--text-tertiary` | `#9a9895` | Texto apagado, placeholders de label |
| `--text-placeholder` | `#c5c3be` | Placeholder dentro de inputs |
| `--shadow-sm` | ver código | Cards en reposo |
| `--shadow-md` | ver código | Cards en hover |
| `--shadow-lg` | ver código | Preview card aurora |

### Dark mode

Cada tool incluye dark mode toggle. Todas las variables se redefinen en `[data-theme="dark"]`. El token `--text-placeholder` en dark es `#3a3848`.

### Gradiente de texto — canon

Para palabras clave en títulos (`page-title span`) y hero (`hero-title span`):

```css
background: linear-gradient(135deg, var(--purple) 0%, #7b5ea7 50%, var(--orange) 100%);
-webkit-background-clip: text;
-webkit-text-fill-color: transparent;
background-clip: text;
color: transparent;
display: inline-block; /* requerido para que el clip funcione bien */
```

### Gradiente de icono de card

Para el `.tool-card-icon` en cards activas del Hub y Frame index:

```css
background: linear-gradient(135deg, var(--purple) 0%, #5a3a8a 100%);
```

---

## Componentes del sistema

### Header

Dos variantes según nivel de navegación:

**Hub** — solo brand name:
```html
<span class="brand-name">Stampede</span>
```

**Tool de primer nivel** (Tag, Scale, Frame index) — brand + sep + tool name:
```html
<a href="../index.html" class="brand-name">Stampede</a>
<div class="header-sep"></div>
<span class="header-tool">NombreTool</span>
```

**Tool de segundo nivel** (Frame · JAC4) — breadcrumb completo:
```html
<nav class="breadcrumb">
  <a href="../../index.html" class="bc-link">Stampede</a>
  <span class="bc-sep">/</span>
  <a href="../index.html" class="bc-mid">Frame</a>
  <span class="bc-sep">/</span>
  <span class="bc-current">JAC4</span>
</nav>
```

### Preview card — Aurora

Gradiente animado con orbes flotantes. Presente en Tag y JAC4.

```css
/* Fondo animado */
background: linear-gradient(-45deg, #0f0c22, #34275f, #6b3070, #c0451e, #df6f40, #34275f, #0f0c22);
background-size: 400% 400%;
animation: auroraShift 14s ease infinite;

/* Orbe naranja */
background: radial-gradient(circle, rgba(223,111,64,0.38) 0%, transparent 68%);
animation: orbeA 9s ease-in-out infinite;

/* Orbe purple */
background: radial-gradient(circle, rgba(123,94,167,0.42) 0%, transparent 68%);
animation: orbeB 11s ease-in-out infinite;
```

### Botón primario — border gradiente animado

El botón de copiar usa un wrapper con gradiente animado como borde:

```css
.btn-copy-wrap {
  background: linear-gradient(90deg, var(--purple), #8a5fc7, var(--orange), #e8845a, var(--purple));
  background-size: 300% 300%;
  animation: borderFlow 5s linear infinite;
  padding: 1.5px;
  border-radius: var(--radius-pill);
}
```

El botón interno tiene `background: var(--surface)` para crear el efecto de borde.

### Chips — resultado generado

Aparecen al generar un resultado. Animación de entrada:

```css
@keyframes chipIn {
  from { opacity: 0; transform: translateY(4px); }
  to   { opacity: 1; transform: translateY(0); }
}
```

Estilos: fondo semitransparente sobre la preview card aurora, borde sutil, fuente JetBrains Mono 11px.

### Toggles ON/OFF

Color naranja cuando activo (`var(--orange)`), gris cuando inactivo (`var(--surface-3)`). Transición suave 0.22s. Thumb blanco con sombra.

### Shake animation — error sin input

Se aplica al botón de acción cuando se intenta copiar/generar sin datos:

```css
@keyframes shake {
  0%,100% { transform: translateX(0); }
  20% { transform: translateX(-6px); }
  40% { transform: translateX(6px); }
  60% { transform: translateX(-4px); }
  80% { transform: translateX(4px); }
}
```

### Sonido al copiar — Web Audio API

Sin archivos externos. Tres capas: oscilador sine descendente + oscilador triangle + burst de ruido filtrado. Ver `playSeal()` en `tag/index.html` como referencia.

### Live badge — Firebase (pendiente)

Elemento `#liveBadge` presente en Tag pero con `display: none`. Activar cambiando a `display: flex` cuando Firebase esté conectado. El dot pulsa con `@keyframes pulse`.

### Section label con línea

```css
.section-label::after {
  content: '';
  flex: 1;
  height: 1px;
  background: var(--border);
}
```

### Status dot — disponibilidad

Verde `#3ecf6a` para tools activas. Presente en cards del Hub y Frame index.

---

## Tools

### ✅ Tag — v6
**Descripción:** Generador de nombres de archivo con nomenclatura consistente.
**Estructura:** `Cliente_Campaña_Medidas_Modelo_Fecha_Extra1_Extra2_Versión_Iniciales`
**Clientes:** JAC, SHOP-SMALL, FACIL, POC, MERCK + campo libre
**JAC especial:** dropdown de modelo (JAC2, JAC4, Traveler_PHEV, JAC6_PHEV, SUNRAY_CITYCARGO, SUNRAY_PASAJEROS)
**Features:** medidas toggle · extras 1 y 2 · fecha DDMMYY · iniciales auto-uppercase · live preview con chips · historial 10 nombres (localStorage) · sonido al copiar (Web Audio API) · shake si vacío · dark mode

---

### ✅ Scale — v1
**Descripción:** Conversor de unidades reactivo para diseño.
**Unidades:** px · pt · mm · cm · in
**DPI:** presets 72 / 150 / 300 + campo custom (hasta 2400)
**Contexto:** selector RGB / CMYK con recomendaciones dinámicas
**Features:** editas cualquier campo y todos se actualizan · info card con resolución activa y perfil recomendado · dark mode

---

### ✅ Frame — hub
**Descripción:** Selector de campañas para el generador de prompts.
**Campañas activas:** JAC4

---

### ✅ Frame · JAC4 — v1
**Descripción:** Generador de prompts para personas en fondo blanco, campañas JAC.
**Features:** presets de escenario (9 configuraciones) · campo custom · dirección de caminata · paleta de outfit · rango de edad con sliders · nivel de energía · expresión · nota adicional · diferenciación forzada de personas (build, tono, cabello, outfit, accesorio) · shake si vacío · dark mode

---

### 📋 Ideas en lista

| Nombre | Descripción |
|--------|-------------|
| **Ratio** | Derivados de formato para redes desde un tamaño base |
| **Kereo** | Checklist de entrega antes de mandar a cliente/producción |
| **Crono** | Estimador de horas por tipo de pieza para cotizar |
| **Memo** | Notas rápidas por cliente, persistentes en localStorage |
| **Tono** | Generador de guía de tono de comunicación |
| **Grid** | Calculadora de grillas tipográficas |

---

## Integraciones pendientes

### Simple Analytics
Comentado y listo en cada tool. Activar descomentando 2 líneas.
Crear cuenta en: simpleanalytics.com (plan gratis disponible)

### Firebase — usuarios en vivo
Comentado con instrucciones completas en `tag/index.html`.
Requiere cuenta propia o acceso al proyecto de la agencia.
Pasos: console.firebase.google.com → Realtime Database → app web → copiar `firebaseConfig`

---

## Convenciones de desarrollo

- **Un archivo por tool** — todo el CSS y JS va inline en el HTML
- **Sin frameworks** — vanilla JS únicamente
- **Sin dependencias externas** — excepto Google Fonts
- **Partir siempre de `_template.html`** — nunca de cero ni de una tool ya terminada
- **localStorage key** — usar `stampede_vN` donde N es la versión actual
- **Nuevo chat = nuevo contexto** — pegar `_template.html` al iniciar un chat nuevo para una tool

---

*Hecho a mano por alguien que también odia nombrar archivos mal.*
