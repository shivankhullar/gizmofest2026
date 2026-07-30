# GIZMOfest 2026 website

Source for <https://shivankhullar.github.io/gizmofest2026/>.

Plain HTML and CSS — no build step, no dependencies, nothing to install. Edit
`index.html`, commit, push, and the live site updates within a minute or two.

```
index.html    the entire page
style.css     all styling
assets/       logo and icons
programme/    the schedule PDF goes here
.nojekyll     tells GitHub Pages to serve the files as-is
```

## The logo

The header logo is the wordmark, so the `<h1>GIZMOfest 2026</h1>` next to it is
visually hidden rather than deleted — screen readers, search engines, and the
print stylesheet still get the name as text. The image is `alt=""` on purpose:
the `<h1>` already announces the name, so giving the image alt text too would
say it twice.

| file | what it is |
| --- | --- |
| `assets/logo-original.jpg` | the pristine 5400×2400 original, not used by the page — kept for posters and slides |
| `assets/logo.jpg` | 1600×711, 96 KB — what the page actually loads |
| `assets/logo.webp` | same image, 31 KB — modern browsers take this one |
| `assets/favicon.png` | 512×512, the black-hole emblem cropped square out of the logo |
| `assets/apple-touch-icon.png` | 180×180 version of the same |

Two things in `style.css` are sampled directly from the logo and will look wrong
if you swap in different artwork:

- `--header-bg: #13161f` is the logo's own background colour, **exactly**. The
  logo is a JPEG with the dark background baked in, so if this drifts even
  slightly you get a visible rectangle around it. There is deliberately no
  gradient on the header for the same reason.
- `--accent: #ad5e38` is the logo's orange (`#d07143`) darkened until it reaches
  4.57:1 contrast on the page background, which is what WCAG AA needs for text.
  The raw orange is only 3.31:1 and fails, so don't use it for links.

### Replacing the logo

1. Put the new artwork in `assets/`, and regenerate the web-sized copies:
   ```sh
   sips -Z 1600 -s format jpeg -s formatOptions 82 assets/logo-original.jpg --out assets/logo.jpg
   cwebp -q 86 assets/logo.jpg -o assets/logo.webp
   ```
2. Re-sample its background colour and set `--header-bg` to match, or the seam
   will show.
3. If the new logo does **not** contain the event name, remove
   `class="visually-hidden"` from the `<h1>` so the name is visible again.

## Preview locally

Open the file directly — everything works over `file://`:

```sh
open index.html
```

## Fill in the placeholders

Every value still to be filled in is marked `class="tbd"` and renders in grey
italics with a dashed underline. List them all:

```sh
grep -n 'class="tbd"' index.html
```

To fill one in, replace the `TBD — …` text **and** remove the `class="tbd"`
attribute so it stops looking like a placeholder.

The **Scientific Rationale** section is empty on purpose — paste the text into
`<section id="rationale">`, one `<p>…</p>` per paragraph, and delete the
placeholder paragraph. Bullet lists, italics, bold, and links inside that section
are already styled, so no CSS changes are needed.

## Post the programme PDF

1. Save the PDF as `programme/gizmofest-programme.pdf`.
2. In `index.html`, find `<section id="programme">`. Delete the "will be posted
   here" paragraph and uncomment the download link below it (remove the
   `<!--` line and the `-->` line around it).
3. Commit and push:

```sh
git add -A
git commit -m "Post programme"
git push
```

Using that exact filename means you never have to edit the link again — later
versions of the schedule just overwrite the same file.

## Open registration

In `<section id="registration">`, set the button's `href` to the registration
form URL (Google Form, Indico, …) and delete `class="disabled"` from the link so
it becomes clickable.

## Publish changes

```sh
git add -A
git commit -m "Describe the change"
git push
```

GitHub Pages is configured under **Settings → Pages** with source
*Deploy from a branch*, branch `main`, folder `/ (root)`. The repo must stay
**public** for Pages to serve it on a free plan.

## Custom domain (optional)

Add a file named `CNAME` containing just the domain, e.g.
`gizmofest.org`, then point a DNS `CNAME` record at
`shivankhullar.github.io`. Set the domain in **Settings → Pages** as well so
GitHub can issue the HTTPS certificate.
