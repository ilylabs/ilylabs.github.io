# ilylabs.com

The website of the ilylabs research team at École de Technologie Supérieure in
Montréal, Canada. Built with Jekyll and the
[Minimal Mistakes](https://github.com/mmistakes/minimal-mistakes) remote theme,
published at <https://ilylabs.com>.

## Running it locally

```bash
bundle install
bundle exec jekyll serve
```

The site is then at <http://localhost:4000>. `bundle exec` matters — it pins the
gem versions in `Gemfile.lock` rather than whatever is installed globally.

## Layout

Content lives in Jekyll collections, one directory per kind:

| directory | holds |
| --- | --- |
| `_posts/` | dated blog posts |
| `_pages/` | standalone pages |
| `_projects/` | current projects |
| `_pastprojects/` | archived projects |
| `_labosminilipec/` | Labos Mini LIPEC entries |
| `_mcmpodcast/` | podcast episodes (feed: `feed.mcmpodcast.xml`) |
| `courses/` | course material |
| `Drafts/` | work in progress, not published |

Supporting directories are the usual Jekyll ones: `_layouts/` and `_includes/`
for templates, `_data/` for site data, `assets/` for images, CSS and JS.

## Adding content

Add a Markdown file to the matching collection with the front matter that
collection expects — copying an existing entry is the quickest way to get the
fields right. Anything in `Drafts/` is ignored by the build.

## Configuration and deployment

`_config.yml` holds the site settings, including the theme and skin. `CNAME`
pins the custom domain. Pushing to `master` triggers the published build, so
preview locally first.
