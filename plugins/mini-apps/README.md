# mini-apps

Plugin de Claude Code para el ecosistema de mini apps de Coco Mercado: creá, previsualizá y publicá mini apps en la wallet describiéndolas en lenguaje natural.

## Qué incluye

- **MCP `miniapp-publisher`** ([`@cocowallet/miniapp-mcp`](https://www.npmjs.com/package/@cocowallet/miniapp-mcp)): publicar, republicar, listar, iniciar/detener mini apps y manejar el túnel de preview con hot reload.
- **Skill `/mini-apps:frontend-design`**: guía de diseño visual de Anthropic para construir UIs distintivas y production-grade que no parezcan plantilla — dirección estética, tipografía y decisiones deliberadas de layout. Claude también la usa automáticamente cuando diseña UI nueva.
- **Skill `/mini-apps:ui-kit`**: el theme de Coco Wallet para mini apps — colores del design system sobre el theme default de Tailwind, Poppins y tema oscuro. Claude la usa automáticamente al crear o modificar UI; el layout de cada mini app es libre dentro del theme. Los componentes del kit se agregarán en versiones futuras.

## Instalación

### 1. Requisitos (una sola vez)

- **Node ≥ 20** (el MCP corre con `npx`).
- **`cloudflared`** para que el teléfono llegue al preview en vivo:

  ```bash
  brew install cloudflared
  ```

- **Token de publicación** del catálogo de develop — pedile tu `PUBLISH_TOKEN` al equipo de wallet y dejalo en tu shell:

  ```bash
  echo 'export PUBLISH_TOKEN="tok_..."' >> ~/.zshrc && source ~/.zshrc
  ```

  Es el único secreto que necesitás. Sin él, el server MCP no arranca (el plugin lo lee de la variable de entorno, nunca del código).

### 2. Instalar el plugin

Dentro de Claude Code:

```
/plugin marketplace add vickcharles/claude-plugins
/plugin install mini-apps@coco-plugins
```

### 3. Verificar

Reiniciá Claude Code **desde una terminal nueva** (para que cargue el `PUBLISH_TOKEN`) y chequeá:

- `/plugin` → `mini-apps` aparece **enabled**.
- Pedile a Claude: *"listá mis mini apps"* — si responde usando el tool, el MCP conectó.

### 4. Primera mini app

Describila y ya:

> "Creá una mini app llamada **Cafecito** para recibir propinas en USDC."

Claude scaffoldea el starter (ya con el theme de Coco y el SDK), levanta `next dev`, abre un túnel público y la publica en el catálogo de develop. Abrí la wallet (flavor dev) y está en el catálogo. Después iterás editando `localDir` — hot reload, sin re-publicar.

## Troubleshooting

| Síntoma | Causa probable | Fix |
|---------|----------------|-----|
| El MCP no aparece / no conecta | `PUBLISH_TOKEN` no está en el entorno | Verificá con `echo $PUBLISH_TOKEN`; arrancá `claude` desde una terminal nueva |
| `publish` falla con 401 | Token inválido o vencido | Pedí un token nuevo al equipo |
| El preview no abre en el teléfono | Túnel caído (laptop cerrada, red) | Decile a Claude: "reiniciá el túnel de mi mini app" |
| Las skills no aparecen | Sesión vieja | `/plugin` para confirmar versión y reiniciá la sesión |

## Uso típico

- `start_mini_app({name})` — bootstrapea todo: scaffold, install, dev server y túnel. Editás los archivos en tu `localDir` con hot reload en vivo.
- `publish_mini_app` / `republish_mini_app` — publica al catálogo de mini apps.
- `list_mini_apps`, `stop_mini_app`, `restart_tunnel`, `close_tunnel` — gestión del ciclo de vida.

El MCP nunca escribe el código de tu app: es infraestructura de lifecycle (scaffold, server, túnel, publish). El código lo escribís vos.

> Nota: las mini apps corren en **Polygon (137)**, no Base. El preview es efímero — vive mientras tu máquina, el dev server y el túnel estén arriba.
