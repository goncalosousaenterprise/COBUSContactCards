# Cobus Contact Cards

A mobile contact directory for CaetanoBus / Cobus. Clients open one link, browse
people grouped by area, and tap a name to get a contact card with a vCard QR code
they can scan straight into their phone's address book.

The whole site is a single self-contained `index.html`. No build step, no
dependencies, no backend — drop it on any static host.

## Running it

Open `index.html` in a browser, or serve the folder:

```sh
python -m http.server 8000    # then open http://localhost:8000
```

## Editing the contacts

There is deliberately no visible link to the editor — clients must not find it.

**Tap the Cobus logo on the home page five times.** A counter hint appears on the
fourth tap. That opens the editor, where you manage areas, people, roles, photos,
phone numbers and links.

When you are done, press **Save page**. The browser downloads a new `index.html`
with your changes baked into it. Replace the file in this repository and push:

```sh
git add index.html
git commit -m "Update contacts"
git push
```

Your edits are also kept in the browser's local storage as you type, so closing
the tab mid-way does not lose them.

`Copy data (JSON)` and `Import JSON` in the editor move the whole directory
between browsers or copies of the site.

## Going live

Any static host works. For GitHub Pages:

```sh
git remote add origin https://github.com/<user>/<repo>.git
git push -u origin main
```

Then in the repository: **Settings → Pages → Source: Deploy from a branch →
`main` / `/ (root)`**. The site appears at `https://<user>.github.io/<repo>/`.

### Before you make the repository public

This site carries employees' names, photos, work emails and phone numbers. On a
public repository, both the rendered page and the file's git history are readable
by anyone who finds the URL, and past versions stay in the history even after you
remove someone.

`index.html` is served with `<meta name="robots" content="noindex, nofollow">`, so
search engines are asked not to index it — but that is a request, not access
control. Anyone with the link can open it.

If that is not acceptable, use a private repository with GitHub Pages access
control (a paid GitHub plan), or host it behind CaetanoBus's own authentication.

## Photos

Photos are resized in the browser and stored inside `index.html` as data URIs, so
there are no separate image files to manage. Expect roughly 60–120 KB per person;
a directory of 30 people lands around 3 MB, which loads fine but is worth keeping
an eye on.

## The QR code

The QR encoder is written from scratch in the page (byte mode, error level L,
versions 1–20) because the site has no external dependencies. It was validated
against `segno` module-by-module, and every generated code was decoded back with
OpenCV across 31 cases, including 25 realistic vCards and non-ASCII text.

The code contains the full vCard, so scanning it with a phone camera saves the
contact directly — nothing to install.

## Where it also runs

The same file runs as a Claude artifact, where the editor's button says
**Publish** instead of **Save page** and the page updates itself in place for
everyone. The page detects which environment it is in at load time.
