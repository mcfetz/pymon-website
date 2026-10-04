# pymon-website

Source for the pymon monitoring stack landing page, published at
**<https://mcfetz.github.io/pymon-website/>**.

A plain static site: one HTML file, one stylesheet, one SVG. No build step, no
framework, no external requests (no web fonts, no CDN, no analytics). It is
published with GitHub Pages straight from the `main` branch.

## Layout

```
index.html          the entire page
assets/style.css    design tokens, components, responsive rules
assets/logo.svg     wordmark and favicon
```

## Local preview

Any static server works:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

Opening `index.html` directly from disk also works; there is no module loading
and no fetch, so `file://` is fine.

## Editing

- **Colours, spacing, radii** live as custom properties in the `:root` block at
  the top of `assets/style.css`.
- **Copy blocks** are `<pre>` elements with a `.copy` button that copies their
  text content. The button looks up its target by the `data-copy` attribute.
- **New sections** get `id`, and matching entries in `.nav-links` for the header,
  the footer list and the scroll spy. The spy picks up any
  `main section[id]` automatically.
- **Scroll reveal** is opt-in per element via the `.reveal` class, handled by an
  `IntersectionObserver`. It degrades to visible-if-unsupported and is disabled
  entirely under `prefers-reduced-motion`.

## Content accuracy

Everything on the page is derived from the actual implementation, not from
marketing. When changing behaviour, update the page in the same commit:

| Claim on the page | Source of truth |
|---|---|
| Plugin count and names | `pymon-server/plugins/*.py` (excluding `_template.py`) |
| Rule scopes, conditions, fire modes | `pymon-server/rules.py` |
| Environment variables | `pymon-server/docker-compose.yml`, `pymon-web/docker-entrypoint.sh` |
| Feature descriptions | `pymon-server/README.md`, `pymon-web/README.md` |

Note that `pymon-server/README.md` lags behind the code in places. The
`fire=single` row still claims "at most one open alarm per (agent, rule)", while
`rules.py` keys on `(agentid, pluginid, metric)`. The website follows the code.
Worth fixing the README in the same change that touched the behaviour.

## Publishing

Push to `main`. Pages is configured to deploy from the branch root, so there is
nothing to run and nothing to release.