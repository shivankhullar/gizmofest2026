# GIZMOfest 2026 website

Source for <https://shivankhullar.github.io/gizmofest2026/>.

Plain HTML and CSS — no build step, no dependencies, nothing to install. Edit
`index.html`, commit, push, and the live site updates within a minute or two.

```
index.html    the entire page
style.css     all styling
programme/    the schedule PDF goes here
.nojekyll     tells GitHub Pages to serve the files as-is
```

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
