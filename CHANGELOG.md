# Changelog

## 1.4.3 — 2026-11-02

### Changed
- `{source}` save protection counts stored block multiplicity (unauthorised editors cannot multiply an existing block)
- `onContentBeforeSave` skips loading the stored row when submitted text has no `{source` tags
- Code Snippet edit view ACL matches Snippets: create for new, edit for existing
- Head Detect uses framework `Joomla\Http\HttpFactory` (CMS HttpFactory is deprecated toward Joomla 8)
- Code snippets content plugin batch-preloads IDs for the current article (one query when cache is cold)
- Shared `AliasSlug` helper for title/alias normalization (Joomla `OutputFilter` when available; pure Greek/Latin fallback for tests)
- `mod_devartsource` migrated to Joomla 6+ Dispatcher / services provider (J7-ready); render behaviour unchanged
- Install/update notices for retired Social/Open Graph and standalone Source package show only when cleanup actually ran
- Package installer `script.php` split into focused traits under `script/` (same postflight behaviour; shared nested-set menu insert helper)
- Backup import streams JSON row-by-row (no full in-memory decode); soft limit raised to 100 MB (or PHP upload limit)
- Admin CSS WebAsset URIs use media-relative paths (safer under reverse proxy / Cloudflare)
- Content plugins share `AllowedContexts` Support helper for `enabled_contexts` matching (behaviour unchanged; plugins remain self-contained)
- Package update no longer force-enables shipped plugins (preserves intentional disable); install / discover_install still enable them
- Snippet create/edit and backup import require `core.admin` (Configure) on `com_devart_exts`
- Snippet HTML uses iframe-safe Joomla InputFilter whitelist sanitization on save/import (scripts, SVG, event handlers, `javascript:` URLs removed; iframes kept)

### Fixed
- DashboardModel reads `snippets_cache*` config keys correctly
- Code snippet `published` is normalized to 0/1; missing ID on update fails cleanly
- Snippet New/Edit from the list no longer inherits `layout=list` (was 500 “Layout list not found”)
- Snippet edit help no longer claims alias/title support is “coming later” (already supported); shows alias shortcode when set

### Notes
- Snippet HTML is trusted-admin content with Configure (`core.admin`) required for create/edit/import; whitelist sanitizer keeps iframes
- Backup import streams up to 100 MB (or PHP upload limit); prefer separate-scope exports for very large sites — see `docs/BACKUP-LARGE-RESTORE.md`
- Package update preserves intentionally disabled plugins
- Read-only duplicate alias scan: `php scripts/scan-duplicate-aliases.php /path/to/joomla`
- See `docs/CLOUDFLARE-CACHE.md` and `docs/BACKUP-LARGE-RESTORE.md`
- Shortcode syntax unchanged
- Requires Joomla 6.0+ and PHP 8.3.0+

## 1.4.2 — 2026-10-24

### Removed
- Large DevArt branding header image from `mod_devartsource` administrator form (module is part of Exts)

### Notes
- Shortcode syntax unchanged
- Requires Joomla 6.0+ and PHP 8.3.0+

## 1.4.1 — 2026-10-05

### Changed
- Empty {source} allowed_groups is fail-closed at save (Super Users / core.admin only); frontend render unchanged
- Unauthorised savers keep existing {source} blocks when content matches after editor-safe normalization (entities, whitespace, outer `<p>`); new/modified blocks are stripped (CLI skips stripping; API/scheduled tasks with a real identity remain subject to ACL)
- Backup export streams JSON in chunks (no full in-memory accumulation / no `JSON_PRETTY_PRINT`); `buildExport()` retained for compatibility
- Snippet and backup import access validation is fail-closed: unknown view levels are rejected or skipped, never coerced to Public (1)
- Aliases use OutputFilter::stringURLSafe with code-level uniqueness
- Snippets plugin batch-preloads alias/title lookups with single-query fallback for collation mismatches; debug HTML comments for core.admin only
- Editor snippet/code-snippet ajax list endpoints require component manage or content create/edit/edit.own
- Content plugins use typed Joomla 6 content/model/module events
- Content plugins rely on `$autoloadLanguage` (manual constructor language loads removed)

### Fixed
- Code snippets assets served from media/plg_content_devartcodesnippets via WebAssetManager (fixes broken plugins/content/.../media URIs)
- Admin CSS WebAsset URIs use absolute Uri::root()
- Snippet sanitize no longer strips PHP/XML open tags; script stripping remains best-effort
- Backup import hygiene: AUTO_INCREMENT repair after commit, 20 MB limit, viewlevel access normalization, correct package element lookup
- HeadDetect hardens redirects, body size, and timeout
- Editor snippet/code-snippet modals lazy-load via com_ajax with server pagination
- Snippet alias/title batch lookup no longer drops case/accent-variant shortcodes silently
- TinyMCE/entity rewrites no longer cause false stripping of unchanged stored `{source}` blocks

### Removed
- Removed dead administrator Social Share view/template
- Source editor button: “Insert empty wrapper” (use Insert with pasted code)
- Dead provider `devartLog()` helpers; unused Dashboard Social/Open Graph status fields

### Notes
- Behaviour change: empty Source allowed_groups no longer means unrestricted save access
- Behaviour change: Registered users without content write rights cannot enumerate snippet titles via com_ajax
- Behaviour change: Editor save of an article with unchanged admin-inserted {source} preserves those blocks (normalized match)
- Behaviour change: invalid Access on snippet save/import fails closed instead of becoming Public
- Backup page hints separate exports for very large databases; import limit remains 20 MB
- Repo-only quality tooling (PHPUnit / PHPStan / PHPCS) lives under `tests/` and is not packaged — see `tests/README.md`
- Snippet admin editor improvement for rich HTML + iframes tracked in `docs/FOLLOW-UP-SNIPPET-EDITOR.md`
- Shortcode syntax unchanged
- Requires Joomla 6.0+ and PHP 8.3.0+

## 1.4.0 — 2026-09-24

### Changed
- Joomla 7/8 forward-compatibility: plg_content_devartcodesnippets migrated to SubscriberInterface and ContentPrepareEvent
- Replaced Document::addStyleSheet/addScript/addStyleDeclaration with WebAssetManager in admin and code snippets plugin
- Replaced HTMLHelper behavior.* calls with WebAssetManager assets (core, multiselect, form.validate, keepalive)
- Replaced deprecated DatabaseDriver service string with DatabaseInterface::class
- Replaced Factory::getDate() with Joomla\CMS\Date\Date
- SnippetTable constructor type-hint updated to DatabaseInterface

### Fixed
- Code snippets plugin loadAssets now skips safely when Document is unavailable and does not permanently mark assets as loaded
- Content plugins guard language load when Language is not initialised
- Restored missing HTMLHelper imports in admin list/edit/backup templates (Class "HTMLHelper" not found)

### Notes
- No functional shortcode or runtime behaviour changes intended
- No manual database fixes are required after installing this version
- Requires Joomla 6.0+ and PHP 8.3.0+

## 1.3.1

### Added
- Added el-GR, fr-FR, de-DE, es-ES, it-IT, pt-PT, cs-CZ, nl-NL, pl-PL, ru-RU, uk-UA, ja-JP, tr-TR, and zh-CN language packs for mod_devartsource

### Fixed
- Greek Source Code usage note now mentions {source} blocks consistently with other locales

### Notes
- No sprintf/formatting tokens were changed in existing strings
- No manual database fixes are required after installing this version
- Requires Joomla 6.0+ and PHP 8.3.0+

## 1.3.0

### Added
- Added mod_devartsource site module to the package (complementary to article {source} shortcodes)
- Added Source Module administrator submenu and instance management page
- Added Tools hub with JSON backup export/import for Snippets and Code Snippets
- Added read-only Detect Head Scripts scanner for published site menu pages
- Added Article Tools-style dashboard and section hubs for Snippets, Code Snippets, Source Code, Source Module, and Tools
- Added Authors-style Back navigation on Snippets, Code Snippets, and Source Module list views
- Added per-submenu administrator header images for Snippets, Code Snippets, Source Code, Source Module, and Tools
- Added Access fields on snippet and code snippet edit forms
- Added language packs for el-GR, fr-FR, de-DE, es-ES, it-IT, pt-PT, cs-CZ, nl-NL, pl-PL, ru-RU, uk-UA, ja-JP, tr-TR, and zh-CN

### Changed
- First public release since 1.1.8 (intermediate 1.2.x builds were not published)
- Social Share and Open Graph are no longer included; existing Exts Social/Open Graph plugins are fully uninstalled on update — use DevArt Article Tools for those features
- Standalone pkg_devartsource is retired safely: module instances, params, and positions are preserved
- Renamed administrator submenu Source to Source Code; Source Code usage documents editor insert via CMS Content > DevArt Source
- Replaced deprecated Factory::getCache with CacheControllerFactoryInterface across snippet and code-snippet cache paths
- Centralised code-snippet cache clearing in CacheHelper (save, delete, publish, backup import, dashboard action)
- Detect Head Scripts polish: category filters, duplicate grouping, third-party detection, Home/latest-article presets, copy/open/details actions
- Administrator submenu active colour aligned with DevArt Article Tools red theme
- Joomla native updates remain package-only (pkg_devart_exts)

### Fixed
- Admin active submenu now loads DevArt red reliably (absolute media stylesheet) matching Article Tools
- Snippets list footer is centered; removed per-row {snippet id="…"} hint under the title (ID column remains)

### Security
- Snippet persistent cache keys now include authorised view levels
- Snippet cache lifetime now correctly treats configured seconds as minutes for Joomla Cache::setLifeTime()
- Snippets editor button lists only snippets the current user may view
- Code snippets now have per-item access levels; frontend render, editor button listing, and cache keys respect authorised view levels
- Optional allowed user groups for {source}: editor button hidden and {source} blocks stripped on save when the saver is outside the list (empty = unrestricted; Super Users always allowed)
- Code snippet cache is cleared after save, delete, and publish/unpublish
- SnippetsModel list ordering uses a whitelist before SQL ORDER BY
- Codesnippet edit and add/edit controller paths require core.create or core.edit
- Dashboard cache-clear actions require core.manage
- Standalone pkg_devartsource shell is removed without Installer::uninstall(package), preventing accidental destruction of absorbed mod_devartsource instances

### Notes
- Public release replacing the previous public line at 1.1.8
- Content plugin {source} and site module mod_devartsource are complementary surfaces — both remain
- Never uninstall pkg_devartsource via Installer::uninstall(package) — that would destroy module instances
- Snippet, code snippet, and {source} shortcode syntax is unchanged
- Detect Head Scripts is read-only and does not inject or modify scripts
- No manual database fixes are required after installing this version
- Requires Joomla 6.0+ and PHP 8.3.0+
- Recommended update for all DevArt Exts installations on 1.1.8 or earlier

## 1.1.8

### Changed
- Improved installer script to automatically repair Joomla update site configuration
- Improved update server stability across fresh and existing installations
- Improved compatibility with Cloudflare and server-level compression setups
- Standardized package identity and manifest consistency across releases
- Improved Open Graph metadata stability when Joomla Page Cache is enabled
- Improved overall reliability of package updates in production environments

### Fixed
- Fixed Joomla Update system not detecting new versions in some installations
- Fixed incorrect update site registration (type=package to type=extension)
- Fixed update server linking issues between package and Joomla update system
- Fixed inconsistent package identity (packagename mismatch) causing update failures
- Fixed corrupted frontend output under Joomla Page Cache in specific environments
- Fixed edge cases with compressed responses (gzip / double compression)

### Notes
- Removed macOS system files (__MACOSX, .DS_Store, ._*) from package
- Cleaned package structure for consistent deployment
- This release focuses on Joomla update system reliability and production stability
- No manual database fixes are required after installing this version

## 1.1.7

### Changed
- Open Graph metadata now generated strictly from raw Joomla article data (title, introtext, images JSON)
- Open Graph description cleaned from HTML, scripts and rendered plugin output
- Open Graph tags now injected only via Joomla Document API for full compatibility with caching layers
- Improved compatibility with Joomla Page Cache and Cloudflare caching
- Improved stability when Snippets and Source plugins are used inside articles
- Improved handling of article content parsing for metadata extraction

### Fixed
- Fixed Open Graph corruption when Joomla Page Cache is enabled
- Fixed random/garbled characters appearing in og:description under cached responses
- Fixed double-processing issues affecting meta tags generation
- Fixed frontend break in specific cases caused by invalid cached output

### Notes
- This release focuses on cache compatibility and Open Graph stability for production environments
- Recommended setup: Joomla Page Cache ON, Joomla Gzip OFF when server or CDN compression is active

## 1.1.4

### Changed
- Updated package to be fully compliant with Joomla Extensions Directory (JED) requirements
- Cleaned package structure and removed unnecessary system files
- Improved installation consistency across different environments

### Fixed
- Resolved packaging issues that could cause installation inconsistencies

### Security
- Added GPL license declaration in all XML manifest files (JED requirement)
- Added GPL license headers in all PHP files across the package

### Notes
- This is a JED compliance and stability release based on the stable 1.1.3 version
- Import/Export functionality has been temporarily removed and will be reintroduced in a future version

## 1.1.3

### Changed
- Unified {source} parsing for both articles and modules
- Improved compatibility with inline and external JavaScript execution
- Updated all package, component and plugin manifests to version 1.1.3
- Added legacy Code Snippet shortcode support for [codehighlight id=X]
- Snippets editor modal now displays newest items first
- Code Snippets editor modal now displays newest items first
- Improved Code Snippets UI styling (line number separation)

### Fixed
- Fixed DevArt Source not rendering inside Joomla modules
- Fixed {source} shortcode output appearing as plain text in module positions
- Fixed compatibility issues with external widget scripts and iframe embeds

### Notes
- {source} should be used only for trusted code (scripts, iframes, external widgets)
- Legacy [codehighlight id=X] is supported for migration compatibility

## 1.1.2

### Changed
- Improved installation and upgrade workflow
- Improved package manifest structure
- Improved version consistency across package and extensions

### Fixed
- Minor internal fixes for package handling

### Notes
- Users on version 1.1.1 need to install this update manually once. After that, updates will work automatically.

## 1.1.1

### Changed
- Modular Joomla 6 component and plugins package
- Centralized configuration panel
- Editor button integrations
- Improved cache handling stability
- Improved dashboard status reporting
- Improved uninstall behavior with data persistence options
- Added logging support for debugging

### Security
- No script execution allowed in Snippets or Code Snippets
- Raw code isolated in DevArt Source
- ACL and CSRF protection implemented
- Safe database handling
