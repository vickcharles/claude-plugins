# mini-apps

Plugin de Claude Code para el ecosistema de mini apps de Coco Mercado.

## Qué incluye

- **MCP `miniapp-publisher`** ([`@cocowallet/miniapp-mcp`](https://www.npmjs.com/package/@cocowallet/miniapp-mcp)): publicar, republicar, listar, iniciar/detener mini apps y manejar el túnel de preview con hot reload.
- **Skill `/mini-apps:frontend-design`**: guía de diseño visual de Anthropic para construir UIs distintivas y production-grade que no parezcan plantilla — dirección estética, tipografía y decisiones deliberadas de layout. Claude también la usa automáticamente cuando diseña UI nueva.
- **Skill `/mini-apps:ui-kit`**: el UI kit de Coco Wallet para mini apps — tokens de diseño (colores, Poppins, spacing, radius) mapeados 1:1 del design system Flutter, más recetas de componentes (botones, cards, sheets, toasts). Claude la usa automáticamente al agregar o modificar componentes, para que la mini app se vea nativa dentro de la wallet.
- **Skill `/mini-apps:deploy`**: despliega la mini app a producción en el team de Vercel de Coco (un proyecto `miniapp-<slug>` por app) y actualiza el catálogo con la URL fija. Requiere `VERCEL_TOKEN` y `VERCEL_TEAM_ID` — ver Requisitos.

## Requisitos

1. **Node.js** (el MCP corre con `npx`).
2. **Token de publicación** exportado en el entorno. Pedile tu `PUBLISH_TOKEN` al equipo de wallet y agregalo a tu `~/.zshrc`:

   ```bash
   export PUBLISH_TOKEN="tok_..."
   ```

   Sin el token exportado, el server MCP no arranca (el plugin lo lee de la variable de entorno, nunca del código).

3. **Credenciales de Vercel** (solo para deployar a producción con `/mini-apps:deploy`). Pedilas al equipo de wallet junto con el `PUBLISH_TOKEN`:

   ```bash
   export VERCEL_TOKEN="..."
   export VERCEL_TEAM_ID="team_..."
   ```

## Instalación

Dentro de Claude Code:

```
/plugin marketplace add vickcharles/claude-plugins
/plugin install mini-apps@coco-plugins
```

Reiniciá la sesión y verificá con `/plugin` que `mini-apps` aparezca habilitado. Los tools del publisher quedan disponibles como `mcp__plugin_mini-apps_miniapp-publisher__*`.

## Uso típico

- `start_mini_app({name})` — bootstrapea todo: scaffold, install, dev server y túnel. Editás los archivos en tu `localDir` con hot reload en vivo.
- `publish_mini_app` / `republish_mini_app` — publica al catálogo de mini apps.
- `list_mini_apps`, `stop_mini_app`, `restart_tunnel`, `close_tunnel` — gestión del ciclo de vida.

El MCP nunca escribe el código de tu app: es infraestructura de lifecycle (scaffold, server, túnel, publish). El código lo escribís vos.

> Nota: las mini apps corren en **Polygon (137)**, no Base.
