# AGENTS.md

This file provides guidance to AI coding agents when working with code in this repository.

## What this repository is

This is the Markdown source for the Namazu Elements manual, published at namazustudios.com/docs via BetterDocs. It is a **bi-directional mirror**: edits on the live site sync back here, and PRs merged here sync out to the live site. There is no build system, package manifest, linter, or test suite; the repository is content only, and "correct" means "renders and links correctly on the live docs site."

The manual documents the [Namazu Elements](https://github.com/NamazuStudios/elements) engine. When writing or updating a page, cross-reference the actual engine source there rather than trusting older manual pages or assumptions, since the source is the ground truth for API shapes, configuration keys, and behavior.

## Repository layout

All pages live flat under `site/`, one Markdown file per page, filename matching the page's URL slug (e.g. `leaderboards.md`, `crossfire-protocol-reference.md`). Release notes follow the pattern `<major>-<minor>-release-notes.md` (e.g. `3-8-release-notes.md`). Diagram sources and rendered images live in `site/images/`.

Live URLs are nested by category (e.g. `/docs/namazu-elements-core/features/web3/smart-contracts/`), so the join key between a live page and its file is the **last path segment**.

## Taxonomy: every page needs a category and at least one tag

A documentation page is incomplete unless it is filed under **exactly one `doc_category`** and carries **at least one `doc_tag`**. Do not open a PR for a new or relocated page without both set.

These are assigned in WordPress, not in this repository. The `.md` file carries no category or tag metadata, so confirm the assignment on the live page (or via `/wp-json/wp/v2/docs`) before opening a PR, and say so in the PR description.

### Choosing a category

Prefer an existing term over creating a new one. There are 31 terms in the live `doc_category` taxonomy, nested beneath these 13 top-level parents:

| Parent | Existing child terms |
|---|---|
| `add-ons` | `commerce` (and `commerce/stripe`), `container-on-demand`, `crossplay-gaming-backend`, `roblox` |
| `configuration` | `batch-samples` |
| `custom-code` | none |
| `fundamentals` | none |
| `game-engine-client-support` | `gamemaker`, `unity` |
| `general-concepts` | none |
| `getting-started` | none |
| `namazu-agent` | none |
| `namazu-cloud` | none |
| `namazu-elements-core` | `advanced-operators`, `features`, `features/web3`, `features/web3/smart-contracts`, `queries`, `user-authentication-sign-in` (and `user-authentication-sign-in/auth-schemes`) |
| `releases` | none |
| `restful-apis` | none |
| `troubleshooting-guide` | `local-sdk`, `community-edition` (and `community-edition/running-in-the-ide`) |

Use the most specific existing term that fits. If no term fits, propose one in the PR rather than creating it silently.

### Choosing a tag

<!-- TODO(docs): tag vocabulary is still being agreed. The three terms below are the only ones that exist today, and they are too coarse to tag every page. Replace this list with the agreed vocabulary, and record how many tags a page should carry. -->

At least one tag is required. The live `doc_tag` taxonomy currently contains only three terms:

- `core-features`
- `releases`
- `web3`

### Glossaries

A third taxonomy, `glossaries`, is also attached to the `docs` post type. It does **not** appear in the site's XML sitemaps, so it is invisible to sitemap-based tooling and must be read through the WordPress REST API (`/wp-json/wp/v2/taxonomies`, `rest_base` `glossaries`).

## File format: WordPress Gutenberg blocks, not plain Markdown

Page bodies are HTML wrapped in WordPress Gutenberg block comments, e.g.:

```html
<!-- wp:paragraph -->
<p>Some text with <strong>bold</strong> and <code>inline code</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>An item</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
```

This structure is required for the BetterDocs round-trip sync to work, so do not flatten it to plain Markdown. Match the existing block-comment/tag conventions (`wp:paragraph`, `wp:heading`, `wp:list`, `wp:list-item`, `wp:quote`, `wp:table`, `wp:image`, `wp:separator`, etc.) when adding or editing content. Only the page's `<h1>` title sits outside a block comment.

## Internal links

Cross-references to other manual pages use a bare slug matching the target file's name, with no `.md` extension, e.g. `href="oidc"` or `href="account-linking"`. Some older pages have legacy `../` prefixes left over from a previous site hierarchy. These are inconsistent artifacts, not a convention to copy; write new internal links as a bare slug.

## Diagrams

Diagrams are authored as Mermaid source files in `site/images/*.mmd` and rendered to matching `site/images/*.svg` files. A page embeds a diagram as:

```html
<figure class="wp-block-image"><img src="images/<name>.svg" alt="<description>"/></figure>
```

When a diagram's SVG hasn't yet been rendered and uploaded to the WordPress media library, it is preceded by a `<!-- TODO(docs): render source at images/<name>.mmd, upload the SVG to the WP media library, then replace the src below with the hosted URL -->` comment, and the `img src` points at the local repo-relative path as a placeholder. Once rendered and uploaded, the `src` is swapped to the hosted `namazustudios.com/wp-content/uploads/...` URL and the TODO comment is removed. See `oidc-login-for-thick-clients-browser-redirect-flow.md` for the after state.

## Writing style

Do not use em dashes in manual prose. Use a comma, parenthesis, colon, or a separate sentence instead.
