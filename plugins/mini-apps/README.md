# mini-apps

Plugin de Claude Code para el ecosistema de mini apps de Coco Mercado.

## Qué incluye

- **MCP `miniapp-publisher`** ([`@cocowallet/miniapp-mcp`](https://www.npmjs.com/package/@cocowallet/miniapp-mcp)): publicar, republicar, listar, iniciar/detener mini apps y manejar el túnel de preview con hot reload.
- **Skills** (próximamente): se invocan como `/mini-apps:<nombre>`.

## Requisitos

1. **Node.js** (el MCP corre con `npx`).
2. **Token de publicación** exportado en el entorno. Pedile tu `PUBLISH_TOKEN` al equipo de wallet y agregalo a tu `~/.zshrc`:

   ```bash
   export PUBLISH_TOKEN="tok_..."
   ```

   Sin el token exportado, el server MCP no arranca (el plugin lo lee de la variable de entorno, nunca del código).

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
