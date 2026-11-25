---
name: elementor-html-to-page
description: Build a reference HTML page (or a React/__bundler bundle) as a Draft page in Elementor through the Elementor MCP (atomic e-* widgets), matching it about 95% on the first pass with the fewest tokens. Use when the user gives an HTML/design file and asks to build, recreate, or convert it in Elementor. Builds layout and content only. Never does SEO, forms, anchors, header/footer, or publishing.
---

# Elementor MCP: reference HTML to Draft page

Goal: about 95% visual match, as few tokens as possible. Most tokens are lost to fix loops and big outputs. Follow the order below.

## Scope (hard rules)
- Build layout and content only. Do NOT do SEO (title, meta, schema, canonical), forms, HTML anchor IDs, header/footer, or topbar/sticky bars. The user does those by hand.
- Keep the page a Draft. Never publish or delete without approval.
- At the end, list what is left for the user: form, anchors, SEO, real images.

## Before building: online or local? (heavy pages only)
After unpacking the reference (step 0.1), size the page: rough custom CSS size, scroll-driven animations (view timelines, scroll JS), element count.
- **Light page** (little CSS, no scroll animations): build online right away. Don't ask, don't mention the local option.
- **Heavy page** (CSS above ~15KB, OR scroll animations, OR 400+ elements): stop and let the user choose, in their language, using the wording and time table in the `elementor-local-first` skill (`references/setup-guide.md` › "The choice"). Explain plainly: what the local route is, what it changes (fast fix rounds, no 503s, one upload at the end), what it costs (one-time setup, and the first heavy page saves little), and that the numbers are estimates unless `references/timings.md` has measured ones. If `.local-first.json` already exists for this site, say setup is already done. Ask with AskUserQuestion: "On my machine first" / "Online only". Then follow their choice and don't ask again in that session.
- Never offer this for blog posts, small edits or light pages.
- **`elementor-local-first` is not installed?** Don't offer the choice. Build online, and say in one line that heavy pages go faster with that companion skill if they add it.

If the user picks online for a heavy page: test every fix in the browser first (CSSOM or an injected style), then save the CSS in ONE call. Don't resend the whole stylesheet per fix.

## 0. Prepare (once)
1. If the file is a bundle (`__bundler/template`), unpack the template JSON with Python (`python -I`), strip the `@font-face` blocks, and read it. Serve a static copy on localhost as the reference.
2. Probe the MCP in one go: `list-resources`, `read-resource` for global-variables and global-classes, `list-components`. If the schema tools (`get-widget-schema`, `list-widget-schemas`) throw errors, do not retry them. Use `build-composition` with `dry_run` instead.
3. Measure the reference ONCE with one JS call per width (1920, 1280, 375): height of each section, page width, h1 size. Save this table. This table is the review. Do not re-measure the reference later.
4. From the template, list every pattern that repeats 3 or more times (card, list row, bullet, label, button). Each becomes a global class BEFORE building. Create all classes and the missing color variables in one `manage-classes` and one `manage-global-variable` call.

5. Write the review plan down now, before building: the expected height of every section at the 3 widths (the saved table). After the build, the review is only a comparison against this table. No other reference reads.
6. If the user built the design in Claude Design, ask them (or check the file) for: one repeated style per pattern (card, button, heading, list row) with no per-instance value changes, and a list of the colors, fonts and sizes used. Fewer unique styles means fewer global classes and fewer tokens.

## Widgets seen in the Elementor editor (Atomic Elements)
Div block, Flexbox, Grid, Tabs, Accordion, Loop, Heading, Image, Paragraph, SVG, Button, YouTube, Divider, Video, Background Video. No list widget and no form widget. Components and Globals tabs exist. Untested ideas (try on a new page and note the result here): native Grid (`e-grid`) instead of `display:grid` on a div-block, `e-divider` for rules, Components for repeated cards (`list-components`, `manage-component`), saved blocks from the other Elementor MCP.

## 1. Page setup (once)
`create-page`, then `update-page-settings`: `template: "elementor_header_footer"` and `custom_css` that:
- removes the theme container limit and padding (`body.page-id-X` and `body:has(.elementor-X)` selectors) and sets `overflow-x:hidden`;
- fixes the theme's mobile root font size (Astra shrinks it to about 91%): `@media(max-width:921px){html:has(.elementor-X){font-size:100%!important}}`.

## 2. Build (3 to 4 sections per `build-composition` call)
- Never set styles per element when a global class can do it. Attach classes through `classes`. Inline `style` only for what is unique.
- Every container (`e-div-block`, `e-flexbox`) must set `padding` explicitly. The default is 10px and it silently adds height. Also set `flex-direction` and `min-width:0` on tiny elements (dots, bars).
- Flex rows with text: give the text `flex:1;min-width:0`, or it shrinks and wraps.
- `display:grid`, `grid-template-columns`, `aspect-ratio`, `clamp()`, `&::before/&::after` and `@media(--mobile)` all work in `style`.
- Not supported or unreliable: single-side border colors (use `border` plus inset box-shadow), two-value gap, gradients, inline `style` inside text (use `& span{}` on the element), `animation`.
- Set `font-family` and `letter-spacing` on every paragraph. Unclassed paragraphs inherit the theme's.
- Colored spans: put `<span>` in the text and style with `& span{color:..}`. Links in text: `<a>` in the paragraph plus `& a{}`.
- Clickable card: `link` in `element_config` of the `e-div-block`. Button: `e-button` with `text` and `link`.
- Lists: there is no list widget. Use a flex column of paragraphs with a `::before` dot (put the dot in a global class).
- FAQ: native `e-accordion` > `e-accordion-item` > `e-accordion-item-header` > `e-accordion-item-title` > `e-heading`, plus `e-accordion-item-content`. Closed by default: `manage-elements` settings `{"default_state":"all_collapsed"}` on the accordion. The icon is a `&::after` on the header.
- Images: no real images unless they are in the Media Library (`list-assets`). Build the placeholder box from the reference.
- Mobile text: the reference often changes body size at its breakpoint. Put that in the shared class with `@media(--mobile)`.

## 3. Review (once, after everything is built)
1. Run one `manage-elements` no-op save (for example `style:"padding:0;"` on a small element). The generated CSS lags one save behind, so without this the preview shows stale styles.
2. `create-preview-link` (it expires in about 5 minutes, so create it right before opening). Open it and run ONE JS call per width that returns all section heights, then compare with the saved table.
3. Fix only sections that are off by more than 3%. Find the cause with one JS call on that section's children. Do not dump page structure.
4. Take screenshots only at the very end, one per width, scaled to 0.5.

## Token rules
- Never call `get-page-structure` with `include_content` on a big element. Skeleton only, or one small element.
- Use element ids from your own `resolved_xml`. Do not re-read the page.
- When patching with `manage-elements`, a patch of `padding-left` can wipe the padding shorthand. If you must change a shorthand, send the full CSS with `style_apply_mode:"replace"`.
- Batch many `manage-elements` operations in one call (up to 50).
- Do not review per batch. Do not take screenshots at 3 widths per batch.
- Start a new chat for each page. Long chats make every call more expensive.

## Final report (short)
Built sections, unsupported items (form, anchors, SEO, images), remaining numeric differences, the edit link, token usage.
