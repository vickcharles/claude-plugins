---
name: ui-kit
description: UI kit de Coco Wallet para mini apps — tokens de diseño (colores, tipografía, spacing, radius) y recetas de componentes. Usar SIEMPRE que se cree, agregue o modifique UI en una mini app de Coco — componentes, pantallas, botones, cards, listas, modales, formularios — para que se vea nativa dentro de la wallet. También cuando el usuario diga "usa el UI kit", "que se vea como la wallet", "estilo Coco".
---

# UI Kit de Coco Wallet — Mini Apps

Las mini apps corren dentro de un WebView de la wallet de Coco (app Flutter, tema oscuro). El objetivo es que la UI **no se distinga** de las pantallas nativas de la wallet. Todos los tokens de abajo están mapeados 1:1 del design system Flutter (`DsColors`, `DsTypography`, `DsSpacing`, `DsRadius`).

## Reglas de oro

1. **Tema oscuro únicamente.** Fondo `#181818`, nunca fondos claros ni modo claro.
2. **Poppins siempre.** Es la única fuente del producto (ya viene cargada en el scaffold).
3. **No inventes colores.** Usá los tokens de la tabla. Si necesitás un color que no existe, elegí el token semántico más cercano — no un hex nuevo.
4. **El scaffold ya trae los tokens** en `tailwind.config.ts` y clases de componente en `src/app/global.css`. Usá las clases Tailwind semánticas (`bg-card`, `text-txt-secondary`, `bg-green`) en vez de valores arbitrarios (`bg-[#222222]`).
5. **Verde = acción primaria.** Un solo CTA verde por pantalla; lo secundario va outlined o en superficie neutra.
6. **Touch targets ≥ 44px** de alto; es una app móvil dentro de WebView.

## Colores (clases Tailwind del scaffold)

| Uso | Clase | Hex | Origen Flutter |
|-----|-------|-----|----------------|
| Fondo de página | `bg-bg` | `#181818` | `surfaceDarkest` |
| Superficie elevada | `bg-surface` | `#2C2D2F` | `surfaceDark` |
| Card | `bg-card` + `border-card-border` | `#222222` / `#333333` | `cardBg` / `cardBorder` |
| Divider | `divider` | `#353434` | `cardDivider` |
| Acción primaria / éxito | `bg-green` | `#00AF71` | `buttonPrimaryBg`, `statusSuccess` |
| Verde claro (conectado, acentos) | `text-green-light` | `#24D683` | `textConnected` |
| Fondo de éxito resaltado | `bg-green-highlight` | `#0A2E1F` | `highlightedSuccess` |
| Error | `text-error` | `#EE4F39` | `statusError` |
| Warning | `text-warning` | `#E48B1E` | `statusWarning` |
| Info | `text-info` | `#89AEE5` | `statusInfo` |
| Texto principal | `text-txt` | `#FFFFFF` | `textPrimary` |
| Texto secundario | `text-txt-secondary` | `#C1C1C1` | `textSecondary` |
| Texto atenuado | `text-txt-muted` | `#8A9E96` | `textMuted` |
| Texto sutil (subtítulos de card) | `text-txt-subtle` | `#9CA3AF` | `cardSubtitleLight` |
| Texto deshabilitado | `text-txt-disabled` | `#71717B` | `infoCardText` |
| Botón deshabilitado | `bg-btn-disabled` | `#2C2D2F` | `buttonDisabledBg` |
| Destructivo (outlined) | `border-btn-destructive text-btn-destructive` | `#F3392E` | `buttonDestructive*` |
| Acento focus (OTP, inputs especiales) | `purple` | `#A257FA` | `focusPurple` |

Tokens útiles que no están en el tailwind config (usá arbitrary value con el hex exacto): skeleton base `#2C2D2F` / highlight `#3A3D42`, drag handle de sheet `#3D3D3D`, fondo de fila de lista `#1E1E1E`, subtítulo de fila `#9A9E9B`.

## Tipografía

Poppins, con esta escala (la wallet usa base 375px):

| Rol | Tamaño | Peso |
|-----|--------|------|
| Título de pantalla | 20–24px | 700 |
| Título de sección / card | 16px | 600 |
| Cuerpo | 14px | 400 |
| Secundario / caption | 12px | 400 |
| Monto grande (hero) | 32–36px | 700 |

En el WebView los `font-weight` de Tailwind a veces se ignoran — el scaffold ya fuerza `.font-bold/.font-semibold/.font-medium` con `!important` en `global.css`; usá esas clases.

## Spacing y radius

- Spacing (`DsSpacing`): clases `ds-xs`(4) `ds-sm`(6) `ds-md`(8) `ds-lg`(12) `ds-xl`(16) `ds-xxl`(20). Padding horizontal de página: 16px (`px-ds-xl`).
- Radius (`DsRadius`): `rounded-xs`(4) `rounded-sm`(8) `rounded-md`(10) `rounded-lg`(12) `rounded-xl`(16), `rounded-pill`(100px). Cards usan `rounded-xl`; botones son pill.

## Recetas de componentes

### Botón primario
Usá la clase del scaffold — replica exacto el `DsButton` de Flutter:
```html
<button class="btn-green w-full">Confirmar</button>
```
(50px de alto, pill radius 30px, verde `#00AF71`, texto `#F5F7F9` 16px/600; disabled se pone `#2C2D2F` automáticamente.)

### Botón secundario (outlined)
```html
<button class="w-full h-[50px] rounded-pill border border-green text-green font-semibold text-[16px] bg-transparent active:opacity-85">
  Cancelar
</button>
```

### Card
```html
<div class="bg-card border border-card-border rounded-xl p-ds-xl">...</div>
```

### Fila de lista (estilo "Mis monedas")
```html
<div class="flex items-center gap-3 bg-[#1E1E1E] rounded-lg px-ds-xl py-3">
  <div class="w-10 h-10 rounded-full bg-surface" />
  <div class="flex-1 min-w-0">
    <p class="text-txt text-[14px] font-medium truncate">Título</p>
    <p class="text-[#9A9E9B] text-[12px]">Subtítulo</p>
  </div>
  <p class="text-txt text-[14px] font-semibold">Valor</p>
</div>
```

### Bottom sheet / modal
Fondo `#181818`, esquinas superiores `rounded-t-xl`, drag handle centrado (`w-10 h-1 rounded-pill bg-[#3D3D3D]`), backdrop con `backdrop-blur-modal` + negro semitransparente. Entra con `animate-slide-up` (ya definida en `global.css`).

### Toast / feedback
Card oscura (`bg-card border-card-border rounded-lg`) con `animate-toast`. Éxito: ícono/texto en `green`; error: en `error`.

### Skeleton de carga
```html
<div class="animate-pulse rounded-lg bg-[#2C2D2F] h-12" />
```

### Estados de transacción
Completada `#00AF71` · En progreso `#D9E9FF` · Rechazada `#FF5757` · Incidencia `#FF8C00`. Badge sobre `bg-card`.

## Checklist antes de entregar UI

- [ ] Ningún hex fuera de los tokens (buscá `#` en tu diff y justificá cada uno)
- [ ] Fondo de página `#181818`, sin flashes blancos (el `body` del scaffold ya lo trae)
- [ ] Poppins en todo; pesos con `.font-*` de `global.css`
- [ ] CTA único en verde, pill, 50px
- [ ] Touch targets ≥ 44px y sin hovers como única affordance (es táctil)
- [ ] Texto legible: principal blanco, secundario `#C1C1C1` — nunca gris oscuro sobre `#181818`
