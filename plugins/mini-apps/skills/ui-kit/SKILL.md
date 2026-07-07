---
name: ui-kit
description: Usar SIEMPRE antes de crear, agregar o modificar cualquier UI en una mini app de Coco — pantallas, componentes, botones, cards, listas, modales, formularios, estilos — y al configurar Tailwind o CSS del proyecto. Aplica el theme de Coco Wallet (tokens de colores, tipografía Poppins, spacing, radius, tema oscuro) para que la mini app respete el look de la wallet. También cuando el usuario diga "usa el UI kit", "el theme de la app", "estilo Coco", "que se vea como la wallet".
---

# Theme de Coco Wallet — Mini Apps

Las mini apps corren dentro de un WebView de la wallet de Coco (app Flutter, tema oscuro). Esta skill define el **theme**: los tokens que toda UI debe respetar. No define layouts ni componentes — el diseño de cada mini app es libre y debe tener identidad propia (no imitar las pantallas de la wallet), siempre dentro de estos tokens.

## Reglas

1. **Tema oscuro únicamente.** Fondo de página `#181818`. Nunca fondos claros ni modo claro.
2. **Poppins siempre.** Única fuente del producto, pesos 400/500/600/700.
3. **No inventes colores.** Usá los tokens de abajo; si falta uno, elegí el semántico más cercano — no un hex nuevo.
4. **Verde `#00AF71` = acción primaria y éxito.** Usalo con intención, no como decoración.
5. **Touch targets ≥ 44px**; es una app móvil táctil dentro de WebView.
6. **El layout es tuyo.** Con estos tokens, diseñá una UI con identidad propia para la mini app — no repliques el home ni las pantallas de la wallet.

## Setup (verificar SIEMPRE primero)

El scaffold del starter (`@cocowallet/miniapp-mcp`) puede venir **sin** estos tokens. Antes de escribir UI, revisá `tailwind.config.ts`: si `theme.extend` está vacío, configurá el theme.

**IMPORTANTE:** Next no recarga `tailwind.config.ts` en caliente — después de editarlo, reiniciá el dev server (`stop_mini_app` + `start_mini_app`); si no, las clases de los tokens no compilan y la UI sale sin estilos.

**`tailwind.config.ts`:**

```ts
theme: {
  extend: {
    colors: {
      bg: '#181818',
      surface: '#2C2D2F',
      card: '#222222',
      'card-border': '#333333',
      divider: '#353434',
      green: { DEFAULT: '#00AF71', light: '#24D683', highlight: '#0A2E1F' },
      error: '#EE4F39',
      warning: '#E48B1E',
      info: '#89AEE5',
      txt: '#FFFFFF',
      'txt-secondary': '#C1C1C1',
      'txt-muted': '#8A9E96',
      'txt-subtle': '#9CA3AF',
      'txt-disabled': '#71717B',
      purple: '#A257FA',
    },
    borderRadius: { xs: '4px', sm: '8px', md: '10px', lg: '12px', xl: '16px', pill: '100px' },
    spacing: { 'ds-xs': '4px', 'ds-sm': '6px', 'ds-md': '8px', 'ds-lg': '12px', 'ds-xl': '16px', 'ds-xxl': '20px' },
    fontFamily: { sans: ['Poppins', 'system-ui', 'sans-serif'] },
  },
},
```

**`globals.css`** — asegurate de que el body sea oscuro y Poppins con pesos forzados (el WebView a veces ignora los `font-weight` de Tailwind):

```css
:root { color-scheme: dark; }
body {
  margin: 0;
  font-family: 'Poppins', system-ui, sans-serif;
  background-color: #181818;
  color: #ffffff;
  -webkit-font-smoothing: antialiased;
  touch-action: manipulation;
}
.font-bold { font-weight: 700 !important; }
.font-semibold { font-weight: 600 !important; }
.font-medium { font-weight: 500 !important; }
::-webkit-scrollbar { display: none; }
```

**`layout.tsx`** — cargá Poppins (Google Fonts) y el body oscuro:

```tsx
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap" />
...
<body className="min-h-screen bg-bg text-txt antialiased">
```

## Tokens de color

| Semántica | Clase | Hex | Origen Flutter |
|-----------|-------|-----|----------------|
| Fondo de página | `bg-bg` | `#181818` | `surfaceDarkest` |
| Superficie elevada | `bg-surface` | `#2C2D2F` | `surfaceDark` |
| Card / contenedor | `bg-card` + `border-card-border` | `#222222` / `#333333` | `cardBg` / `cardBorder` |
| Divider | `divider` | `#353434` | `cardDivider` |
| Primario / éxito | `green` | `#00AF71` | `buttonPrimaryBg`, `statusSuccess` |
| Verde claro (acentos) | `green-light` | `#24D683` | `textConnected` |
| Fondo éxito resaltado | `green-highlight` | `#0A2E1F` | `highlightedSuccess` |
| Error | `error` | `#EE4F39` | `statusError` |
| Warning | `warning` | `#E48B1E` | `statusWarning` |
| Info | `info` | `#89AEE5` | `statusInfo` |
| Texto principal | `txt` | `#FFFFFF` | `textPrimary` |
| Texto secundario | `txt-secondary` | `#C1C1C1` | `textSecondary` |
| Texto atenuado | `txt-muted` | `#8A9E96` | `textMuted` |
| Texto sutil | `txt-subtle` | `#9CA3AF` | `cardSubtitleLight` |
| Texto deshabilitado | `txt-disabled` | `#71717B` | `infoCardText` |
| Acento focus | `purple` | `#A257FA` | `focusPurple` |

## Tipografía

Poppins (base de diseño: 375px de ancho):

| Rol | Tamaño | Peso |
|-----|--------|------|
| Título de pantalla | 20–24px | 700 |
| Título de sección | 16px | 600 |
| Cuerpo | 14px | 400 |
| Caption / secundario | 12px | 400 |
| Cifra hero | 32–36px | 700 |

## Spacing y radius

- Spacing: `ds-xs`(4) `ds-sm`(6) `ds-md`(8) `ds-lg`(12) `ds-xl`(16) `ds-xxl`(20). Padding horizontal de página: 16px.
- Radius: `rounded-xs`(4) `rounded-sm`(8) `rounded-md`(10) `rounded-lg`(12) `rounded-xl`(16), `rounded-pill`(100px).

## Checklist antes de entregar UI

- [ ] Ningún hex fuera de los tokens (buscá `#` en tu diff y justificá cada uno)
- [ ] Fondo `#181818`, sin flashes blancos ni modo claro
- [ ] Poppins en todo; pesos con las clases `.font-*` forzadas
- [ ] Verde solo para acción primaria / éxito
- [ ] Touch targets ≥ 44px; texto secundario `#C1C1C1` o más claro sobre fondo oscuro
- [ ] El diseño tiene identidad propia — no es una copia de las pantallas de la wallet
