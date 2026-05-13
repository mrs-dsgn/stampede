# Stampede — Internal Toolset

Kit de herramientas hecho a mano para el equipo de diseño.
Cada tool hace una cosa y la hace bien.

**URL:** maresmares.github.io/stampede
**Stack:** Vanilla HTML + CSS + JS · Single file por tool · GitHub Pages
**Deployment:** drag & drop — sin build process, sin dependencias

---

## Design System

### Tipografía
| Uso | Fuente |
|-----|--------|
| Labels, código, monospace, títulos de tool | JetBrains Mono |
| Body copy, descripciones, botones secundarios | Plus Jakarta Sans |

### Colores
| Token | Valor | Uso |
|-------|-------|-----|
| `--purple` | `#34275f` | Acento primario, brand |
| `--purple-mid` | `#4a3880` | Estados hover purple |
| `--orange` | `#df6f40` | Acento secundario, eyebrows |
| `--orange-light` | `#e8845a` | Estados hover orange |
| `--bg` | `#f2f1ee` | Fondo general (light) |
| `--surface` | `#ffffff` | Cards, header (light) |
| `--surface-2` | `#f7f6f4` | Inputs, badges (light) |
| `--surface-3` | `#eeece8` | Toggles, prefijos (light) |

### Dark mode
Cada tool incluye dark mode toggle. Variables redefinidas en `[data-theme="dark"]`.

### Componentes recurrentes
- **Preview card** — gradiente aurora animado (`auroraShift` 14s) + orbes flotantes
- **Botón primario** — Apple border gradient animado (`borderFlow` 5s), fondo neutro
- **Chips** — aparecen con animación `chipIn` al generarse
- **Toggles** — ON/OFF con transición suave, color naranja cuando activo
- **Efecto texto gradiente** — para denotar palabras clave: `background: linear-gradient(135deg, var(--purple), #7b5ea7, var(--orange))` con `-webkit-background-clip: text`

---

## Estructura de archivos

```
stampede/
│
├── index.html              ← Hub / landing (stampede_hub.html)
│
├── tag/
│   └── index.html          ← Tag — file naming tool (stampede_v5.html)
│
├── scale/
│   └── index.html          ← Scale — conversor de unidades (en desarrollo)
│
└── frame/
    └── index.html          ← Frame — generador de prompts JAC (en desarrollo)
```

---

## Tools

### ✅ Tag — v5
**Status:** Activa
**Descripción:** Generador de nombres de archivo con nomenclatura consistente.
**Estructura de nombre:** `Cliente_Campaña_Medidas_Modelo_Fecha_Extra1_Extra2_Versión_Iniciales`
**Clientes:** JAC, SHOP-SMALL, FACIL, POC, MERCK + campo libre
**JAC especial:** dropdown de modelo (JAC2, JAC4, Traveler_PHEV, JAC6_PHEV, SUNRAY_CITYCARGO, SUNRAY_PASAJEROS)
**Features:**
- Medidas toggle (ON por defecto)
- Campos extra 1 y 2 (OFF por defecto, toggles independientes)
- Fecha en formato DDMMYY, default hoy
- Iniciales auto-uppercase
- Live preview con chips animados
- Historial últimos 10 nombres (localStorage)
- Sonido al copiar (Web Audio API, sin archivos externos)
- Shake animation si copia vacío
- Dark mode

---

### 🔧 Scale — en desarrollo
**Descripción:** Conversor de unidades para diseño. Reactivo — editas cualquier campo y todos se actualizan.
**Unidades:** px · pt · mm · cm · in
**DPI:** presets 72 / 150 / 300 + campo custom
**Contexto:** selector CMYK / RGB (afecta recomendaciones)

---

### 🔧 Frame — en desarrollo
**Descripción:** Generador de prompts para Nano Banana. Específico para campañas de modelos JAC.
**Campañas activas:** ~4 campañas con necesidades de imagen específicas
**Desarrollado en:** chat Sidewalk - Prompt Generator PT.1

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
Comentado con instrucciones completas en cada tool.
Requiere cuenta propia o acceso al proyecto de la agencia.
Pasos: console.firebase.google.com → Realtime Database → app web → copiar `firebaseConfig`

---

## Convenciones de desarrollo

- **Un archivo por tool** — todo el CSS y JS va inline en el HTML
- **Sin frameworks** — vanilla JS únicamente
- **Sin dependencias externas** — excepto Google Fonts
- **Nombres de versión** — `stampede_v5.html` durante desarrollo, renombrar a `index.html` al deployar
- **localStorage key** — usar `stampede_vN` donde N es la versión actual
- **Nuevo chat = nuevo contexto** — pegar siempre el código de referencia más reciente al iniciar un chat nuevo para una tool

---

## Cómo agregar una tool nueva al Hub

1. Crear carpeta con el nombre de la tool en el repo
2. Desarrollar el archivo y renombrarlo a `index.html`
3. En `stampede_hub.html`: cambiar la card "próximamente" correspondiente por una card activa con link `./nombre-tool/index.html`
4. Actualizar este README

---

*Hecho a mano por alguien que también odia nombrar archivos mal.*
