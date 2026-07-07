# coco-plugins

Marketplace de plugins de Claude Code de Coco Mercado.

## Instalación

Dentro de Claude Code:

```
/plugin marketplace add vickcharles/claude-plugins
/plugin install mini-apps@coco-plugins
```

## Plugins disponibles

| Plugin | Descripción |
|--------|-------------|
| [`mini-apps`](plugins/mini-apps/README.md) | MCP y skills para desarrollar y publicar mini apps de Coco Mercado. Requiere `PUBLISH_TOKEN` — ver su README. |

## Actualizaciones

Los updates se publican subiendo la `version` en el `plugin.json` del plugin. Para traerlos:

```
/plugin marketplace update coco-plugins
```

## Contribuir

Cada plugin vive en `plugins/<nombre>/` con su manifiesto en `.claude-plugin/plugin.json` y debe estar registrado en `.claude-plugin/marketplace.json`. Antes de abrir un PR, validá con:

```bash
claude plugin validate plugins/<nombre>
```
