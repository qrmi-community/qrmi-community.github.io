# QRMI Community website

Source for <https://qrmi-community.github.io/>, built with
[Hugo](https://gohugo.io/). There is no theme or Node toolchain: the layouts and
stylesheet live in this repository.

## Run locally

Install Hugo **extended**, version 0.158.0 or newer (CI uses 0.167.0)
([install guide](https://gohugo.io/installation/)), for example:

```bash
brew install hugo          # macOS
sudo snap install hugo     # Linux
```

Then, from the repository root:

```bash
hugo server --renderToMemory --poll 700ms
```

Open <http://localhost:1313/>. The page reloads when you save a file.

- `--poll 700ms` is needed for layout changes: Hugo (checked with 0.126 and
  0.167) misses templates that are replaced rather than rewritten, which is
  how `sed -i` and many editors save. Without it, layout edits only show after
  a restart.
- `--renderToMemory` keeps the server's pages out of `public/`; without it, any
  other `hugo` build or a deleted `public/` replaces the pages the server
  serves, and live reload stops until you restart it.

To build the static site as CI does (into `public/`, or elsewhere with
`--destination`):

```bash
hugo --minify --destination /tmp/qrmi-site
```

## Layout

```
hugo.toml                       Site title, base URL, top menu
content/_index.md               Home page text
content/tutorials/_index.md     Tutorials list page
content/tutorials/*.md          One file per tutorial: details, modules, presenters (front matter) and abstract (body)
content/ecosystem.md            Ecosystem page intro
content/papers.md               Papers page
data/ecosystem.yaml             QRMI projects and integrations, grouped
data/papers.yaml                Paper list, shown on the home page and the papers page
layouts/_default/baseof.html    Page shell: header, nav, footer
layouts/_default/single.html    Plain Markdown pages
layouts/_default/list.html      Tutorials list
layouts/_default/ecosystem.html Ecosystem page
layouts/_default/papers.html    Papers page
layouts/index.html              Home page: upcoming tutorials, past tutorials, papers
layouts/tutorials/single.html   Tutorial page
layouts/robots.txt              robots.txt pointing crawlers at the sitemap
layouts/partials/               Tutorial and paper lists shared between pages
static/css/style.css            All styling (light and dark)
assets/images/presenters/       Presenter photos
assets/images/card-base.png     Link-preview card background (plus circle-mask.png for photos)
```

## Common edits

**Add or change a presenter.** Edit the `presenters` list in the tutorial's
file, e.g. `content/tutorials/sc26.md`:

```yaml
  - name: Jane Doe
    affiliation: Example Lab
    photo: images/presenters/jane-doe.jpg   # optional
    url: https://janedoe.example            # optional, makes the card a link
```

Put a square photo (200×200 is enough) in `assets/images/presenters/`, named
after the presenter. Without `photo`, the page shows the presenter's initials.

**Add a tutorial.** Copy `content/tutorials/sc26.md` to a new file in
`content/tutorials/` and edit it. Set `date` to the tutorial day: the home page
features tutorials dated today or later as upcoming (through the end of their day) and lists the rest under
"Past tutorials". `room`, `audience`, `materials` and `program` are optional.

**Add a paper.** Add an entry to `data/papers.yaml`:

```yaml
- title: Paper title
  year: 2026
  url: https://arxiv.org/abs/XXXX.XXXXX
  venue: arXiv:XXXX.XXXXX
  authors: First Author, Second Author
```

**Add a project to the ecosystem.** Add an item to the matching group in
`data/ecosystem.yaml`, or add a new group. Projects that live on a branch link
to that branch, so update the URL when the branch is merged or renamed.

```yaml
    - name: Project name
      url: https://github.com/org/repo
      tags: [Flux, Kubernetes]    # optional, workload managers it integrates with
      status: under development   # optional badge
      description: One sentence on what it does with QRMI.
```

Each tag is coloured by a `.tag-<name>` rule in `static/css/style.css`
(the name lower-cased, e.g. `.tag-kubernetes`). A new tag needs a color defined there.
If not defined, it will be shown in a muted color.

**Add a regular page.** Create a Markdown file under `content/`, e.g.
`content/getting-started.md` with a `title` and `description` in the front matter. It renders with
`layouts/_default/single.html`. To add it to the top menu, add a
`[[menu.main]]` entry with `pageRef` in `hugo.toml`.

**Descriptions and search.** Every page needs a `description` in its front
matter, about 160 characters at most. It is used for the search-result snippet
and for link previews (Open Graph and Twitter cards). Each page gets a 1200×630 preview image,
generated at build time by `layouts/partials/social-card.html` from the title,
`event`, `city`, `when` (or `description`) and presenter photos. Tutorial pages also emit
schema.org `Event` data from their `date`, `presenters` and optional `city`.

**Privacy.** The site sets no cookies, runs no analytics and loads nothing
from other domains. The theme toggle stores one `theme` value in local storage.
The footer states this, and notes that GitHub may collect visitor data as the
host. Anything that
changes it (analytics, embeds, external fonts or scripts) must update the
footer statement too.

**Theme.** Colours are CSS variables using `light-dark()` in
`static/css/style.css`. They follow the system theme until the header toggle
sets `data-theme` on `<html>`.

**Links.** Build internal links in templates with `.RelPermalink` or `relURL`,
never a hard-coded `/path`, so the site still works under a sub-path (forks,
previews). In Markdown, use `{{< relref "page" >}}`.

Upcoming and past tutorials are decided when the site is built, so a tutorial
moves to "Past tutorials" on the next deploy after its date.

## Deployment

`.github/workflows/hugo.yml` builds and deploys to GitHub Pages on every push to
`main`, with Hugo 0.167.0; bump `hugo-version` there when using newer Hugo
features. In the repository settings, set **Pages → Source** to **GitHub
Actions**. Open a pull request to propose changes.
