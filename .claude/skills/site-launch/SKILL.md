---
name: site-launch
description: Launch a Panelwalk game on this studio site (panelwalk.github.io) once its itch.io page is public - the LAUNCH markers, the og image, screenshots, itch links, credit line and JSON-LD in both languages, sitemap.xml, previews, Lighthouse, link checks, the founder-approved merge to main and the Google Search Console steps. Use it whenever the user wants to add a released game to the site, update the site for a launch, change the link preview or og image, or check the live site after a release, even if they only say "put it on the site".
---
<!-- Canonical source: panelwalk/panelwalk.github.io (this repo). Split from the launch-comms skill in panelwalk/studio, which keeps the launch posts and artist outreach. -->

# Site launch

This covers the studio site for a game whose itch.io page is ready. The template's `itch-release` skill covers the itch page, and the studio repo's `launch-comms` skill covers the launch posts and artist outreach.

The worked example is the first launch, on branch `launch-ice-cream-palace`.

## Hard rules

- **This repo is public, and `main` is live.** GitHub Pages serves `main` directly, with `.nojekyll`, so every committed file is public, including this one. Keep drafts, previews and notes in the studio repo (`~/Projects/playable-comics`), never here.
- **Merging to `main` publishes.** Prepare each release on a branch `launch-<slug>`. Merge it only after the itch page is public and the founder OKs that merge.
- **Never show another living artist's work.** The site shows only games that are released. An artist's unreleased adaptation stays off the site: no art, names, titles or hints.
- **Make honest claims only:** no player numbers, ratings or turnaround promises, and no "fully mobile" unless dragging by touch was tested on a real phone.
- **Order.** The itch page is public → merge the site branch → launch posts (`launch-comms`) → Search Console → artist outreach (`launch-comms`). Each step links to the one before it, so a step that runs early points at a 404 or a draft.

## 1. On the branch

1. **The `LAUNCH:` markers.** Both `index.html` and `he/index.html` carry them (`grep -rn "LAUNCH:" --exclude-dir=.git .`). On the first launch they mean:
   - remove `<meta name="robots" content="noindex">`
   - swap the og image for the launched game's art

   Remove each marker once it's done. On later launches there may be none left, so add the new game alongside the existing one.
2. **The og image (1200×630).** Render it with `~/Projects/playable-comics/social/make-og-launch.mjs`.
   - **Edit the script first.** The title text and the credit line are hard-coded for the first game. It also loads puppeteer from `~/Projects/comic-game-template/node_modules` via a hard-coded path, and needs Google Chrome and network access for the fonts.
   - **Convert the PNG to a JPEG** (about 1 MB becomes about 240 KB): `ffmpeg -i /tmp/og.png -q:v 3 og-image.jpg`
   - **Update the tags.** Update `og:image` and write a descriptive `og:image:alt`, in both languages.
3. **Content.**
   - **Screenshots:** 3 of them, as `.webp` in `img/`, with `width` and `height` set, and real alt text that describes the art and the balloon text.
   - **Links:** a "Play … free on itch.io" button, plus a one-line "Here to play?" link near the top.
   - **The credit line** for the art.
   - **JSON-LD:** add a `VideoGame` node to the `@graph`, linked to the `Organization`.
   - **Hebrew:** mirror all of it in `he/index.html`. Use a plain hyphen in number ranges, because an en dash flips in RTL.
4. **`sitemap.xml`.** If you added pages, add them here with their `hreflang` alternates. `robots.txt` already points to the sitemap.
5. **Check the result.**
   - Render previews at desktop width and at 375 px, in light and dark, for both languages. Save them to `~/Projects/playable-comics/pitch/site-preview/<launch>/`, not to this repo.
   - Run Lighthouse SEO on a local copy.
   - Click every link.

## 2. After the merge

The founder approves the merge.

- Check the live pages and the link preview. Paste the URL into a Bluesky draft (don't post it) or into a card validator.
- **Google Search Console** is the founder's step, signed in:
  1. Add a **URL-prefix** property for `https://panelwalk.github.io/`.
  2. Verify it.
  3. Submit `sitemap.xml`.

  Tell them the steps. Don't do them yourself.

## Final checklist

- [ ] The itch page is public and plays logged out
- [ ] `LAUNCH:` markers handled, og image and alt text updated, screenshots, links, credit, JSON-LD and Hebrew done
- [ ] `sitemap.xml` updated if pages were added
- [ ] Previews (both languages, both widths, light and dark), Lighthouse SEO and every link checked
- [ ] Merged to `main` only with the founder's OK; the live pages and link preview are checked
- [ ] Search Console steps handed to the founder
