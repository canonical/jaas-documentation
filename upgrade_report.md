# Upgrade Report

## Configuration Changes
- **`conf.py`**: Merged the new starter pack configuration with existing project metadata and custom extensions.
  - Updated `extensions` list to explicitly include unbundled extensions.
  - Preserved project-specific settings (`project`, `copyright`, `html_context`, `redirects`, etc.).
  - Updated `exclude_patterns` to exclude `sp-updated` and `upgrade_prompt.md`.
- **`Makefile`**: Replaced with the new version from the starter pack.
- **`.sphinx/`**: Updated tooling configurations (`pa11y.json`, `.pymarkdown.json`, etc.) from the starter pack.

## Dependency Updates
- **`requirements.txt`**: Updated to include explicit dependencies for extensions that were previously bundled in `canonical-sphinx`.
  - Added: `myst-parser`, `sphinx-design`, `sphinx-copybutton`, `sphinx-reredirects`, `sphinx-tabs`, `sphinxcontrib-jquery`, `sphinxext-opengraph`, `sphinx-terminal`, etc.
  - Preserved project-specific dependencies: `sphinxext-rediraffe`, `sphinx-new-tab-link`, `sphinxcontrib.lightbox2`.

## Syntax Migrations
- **`{terminal}` directive**: Scanned for usage of the `{terminal}` directive. No instances were found in the documentation source files, so no migration was necessary.
- **`{tabs}` and `{note}` directives**: Scanned for usage. `{note}` is used but syntax is compatible. `{tabs}` is not used.

## Cleanup
- Removed `sp-updated/` directory.
- Removed backup directory.
