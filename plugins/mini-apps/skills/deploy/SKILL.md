---
name: deploy
description: Despliega una mini app de Coco a producción en Vercel (team compartido de Coco) y actualiza el catálogo con la URL fija. Usar cuando el usuario diga "deploy", "desplegar la mini app", "subir a producción", "publicar a Vercel", "URL fija de prod", o cuando una mini app terminada necesite pasar de túnel de desarrollo a URL estable.
---

# Deploy de Mini Apps a Vercel

Pasa una mini app del túnel de desarrollo (cloudflared, efímero) a una **URL fija de producción** en el team de Vercel de Coco, y actualiza el catálogo para que la wallet apunte ahí.

## Requisitos (verificar SIEMPRE primero)

```bash
test -n "$VERCEL_TOKEN" && test -n "$VERCEL_TEAM_ID" && echo OK
```

Si falta alguna, detenete y decile al usuario que pida las credenciales al equipo de wallet y las agregue a su `~/.zshrc`:

```bash
export VERCEL_TOKEN="..."
export VERCEL_TEAM_ID="team_..."
```

(`PUBLISH_TOKEN` también debe estar exportado para el paso final de catálogo.)

## Modelo: un proyecto Vercel por mini app

Todas las mini apps viven en el **mismo team** de Vercel, pero cada una es su **propio proyecto** (una app = una URL de prod). El nombre del proyecto es el slug de la mini app con prefijo `miniapp-` (ej. `miniapp-cafecito`), para que en el dashboard del team se distingan del resto de los proyectos.

## Flujo

Desde la raíz de la mini app (donde está el `package.json`):

### 1. Link al proyecto del team (crea el proyecto si no existe)

```bash
npx vercel link --yes --project miniapp-<slug> --scope "$VERCEL_TEAM_ID" --token "$VERCEL_TOKEN"
```

Asegurate de que `.vercel/` esté en el `.gitignore` de la mini app (guarda IDs del team).

### 2. Deploy a producción

```bash
npx vercel deploy --prod --yes --scope "$VERCEL_TEAM_ID" --token "$VERCEL_TOKEN"
```

Si la mini app necesita variables de entorno propias, cargalas antes con
`npx vercel env add <NAME> production --scope "$VERCEL_TEAM_ID" --token "$VERCEL_TOKEN"`.

### 3. Resolver la URL fija

La URL estable de producción es el dominio del proyecto, no la URL del deployment (que tiene hash). Confirmala:

```bash
npx vercel project ls --scope "$VERCEL_TEAM_ID" --token "$VERCEL_TOKEN" | grep miniapp-<slug>
curl -sI https://miniapp-<slug>.vercel.app | head -1
```

Debe responder `200` (o `308` hacia la misma app). Si el nombre global estaba tomado, Vercel asigna un sufijo — usá el dominio real que muestre `project ls`, no asumas.

### 4. Actualizar el catálogo

Con la URL fija verificada, apuntá el catálogo de mini apps ahí usando el MCP del plugin: `republish_mini_app` (o `publish_mini_app` con `source: { url: "https://miniapp-<slug>.vercel.app" }` si es la primera publicación). Esto requiere `PUBLISH_TOKEN`.

### 5. Verificación final

Abrí la URL de prod y chequeá que carga la app (no un 404 de Vercel ni la pantalla de login del team). Reportá al usuario: proyecto, URL fija y que el catálogo quedó actualizado.

## Reglas

- **NUNCA** escribas `VERCEL_TOKEN` en archivos del repo, en `vercel.json`, ni en logs/output visibles. Solo se referencia como `$VERCEL_TOKEN`.
- No toques proyectos del team que no empiecen con `miniapp-` — el token da acceso a todo el team; esta skill solo administra mini apps.
- El deploy es a `--prod` directo; si el usuario quiere probar primero, omití `--prod` y compartí la preview URL sin tocar el catálogo.
- Si `vercel` pide login interactivo, algo está mal con el token — no intentes `vercel login`; reportalo.
