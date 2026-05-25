# olek.dev

Personal blog built with [Astro](https://astro.build) using the [astro-sienna](https://github.com/AnjayGoel/astro-sienna) theme.

## Prerequisites

- [Node.js](https://nodejs.org) (v18 or later)
- [pnpm](https://pnpm.io) — install with `npm install -g pnpm`

## Running the site

```sh
# Start the dev server (live-reloads on every save)
pnpm dev
```

Open http://localhost:4321 in your browser. Stop the server with `Ctrl+C`.

```sh
# Build the production version
pnpm build

# Preview the production build locally
pnpm preview
```

## Adding a new blog post

Create a new `.md` file in `src/content/post/`. The filename becomes the URL slug, so `my-new-post.md` will be available at `/posts/my-new-post/`.

Every post **must** start with a frontmatter block between `---` fences:

```markdown
---
title: "My Post Title"
publishDate: 2026-05-25
description: "A short summary (10-160 characters). Shown in post cards and social previews."
tags: [kotlin, android]
---

Your post content starts here. Write normal Markdown.
```

### Frontmatter fields

| Field         | Required | Description                                               |
|---------------|----------|-----------------------------------------------------------|
| `title`       | yes      | Post title (max 120 characters)                           |
| `publishDate` | yes      | Date in `YYYY-MM-DD` format                               |
| `description` | yes      | Short summary, 10-160 characters                          |
| `tags`        | no       | List of tags, e.g. `[kotlin, android]`                    |
| `draft`       | no       | Set to `true` to hide the post from the production build  |
| `updatedDate` | no       | `YYYY-MM-DD` — shown as "Updated …" on the post page     |
| `coverImage`  | no       | Object with `src` (image path) and `alt` (description)    |
| `ogImage`     | no       | Custom social preview image path (auto-generated if omitted) |

### Markdown cheat sheet

```markdown
## Heading 2
### Heading 3

**bold** and *italic*

[Link text](https://example.com)

![Alt text](./image.png)

- Bullet list
- Another item

1. Numbered list
2. Second item

> Blockquote

`inline code`

​```kotlin
// Code block with syntax highlighting
fun main() {
    println("Hello")
}
​```

Inline math: $E = mc^2$

Display math:
$$
\int_{0}^{1} x^2 dx = \frac{1}{3}
$$
```

You can also use `.mdx` files if you need to embed interactive components — but plain `.md` is enough for most posts.

## Adding images, videos, and other files

There are two places to put files, depending on whether you want Astro to optimize them.

### Option 1: Co-located with the post (recommended for images)

Create a `_assets` folder next to your post and reference files with a relative path:

```
src/content/post/
  my-post.md
  _assets/
    screenshot.png
    diagram.jpg
```

In your Markdown:

```markdown
![Screenshot of the app](./_assets/screenshot.png)
```

Astro will **automatically optimize** these images (resize, compress, convert to modern formats). This is the best option for photos and screenshots.

You can also use this approach for the `coverImage` frontmatter field:

```yaml
---
coverImage:
  src: ./_assets/cover.png
  alt: "Cover image description"
---
```

### Option 2: The `public/` folder (for videos, audio, PDFs, and direct links)

Files in `public/` are served as-is at the site root, with no processing. Use this for files that Astro can't or shouldn't optimize:

```
public/
  files/
    demo.mp4
    podcast.mp3
    resume.pdf
```

Reference them with an absolute path from the site root:

```markdown
[Download my resume](/files/resume.pdf)
```

For **videos** and **audio**, use HTML tags directly in your Markdown:

```markdown
<video src="/files/demo.mp4" controls width="100%"></video>

<audio src="/files/podcast.mp3" controls></audio>
```

Or embed from external services (YouTube, Vimeo, etc.):

```markdown
<iframe width="560" height="315" src="https://www.youtube.com/embed/VIDEO_ID" frameborder="0" allowfullscreen></iframe>
```

### Which option to use?

| File type       | Where to put it                  | Why                                    |
|-----------------|----------------------------------|----------------------------------------|
| Post images     | `src/content/post/_assets/`      | Astro optimizes them automatically     |
| Videos          | `public/files/`                  | Too large for build-time processing    |
| Audio           | `public/files/`                  | Not supported by Astro image optimizer |
| PDFs, downloads | `public/files/`                  | Served as-is, no processing needed     |
| Site-wide images (logo, favicon) | `public/`       | Already there, used globally           |

## Creating standalone pages

Standalone pages (terms of use, privacy policy, etc.) are `.astro` files in `src/pages/`. Each file becomes a route automatically:

1. Create a file like `src/pages/terms.astro`
2. Use this template:

```astro
---
import PageLayout from "@/layouts/Base.astro";

const meta = {
  title: "Terms of Use",
  description: "Terms and conditions for using this website.",
};
---

<PageLayout meta={meta}>
  <section>
    <div class="section-label">Terms of Use</div>
    <div class="prose">
      <p>Your terms content here. Write normal HTML.</p>
      <h2>Section heading</h2>
      <p>More content...</p>
    </div>
  </section>
</PageLayout>
```

The page will be available at `/terms/`. No need to register it anywhere — Astro picks it up automatically.

To add the page to the **navigation menu**, edit `src/site.config.ts`:

```ts
export const menuLinks: { path: string; title: string }[] = [
  { path: "/", title: "Home" },
  { path: "/posts/", title: "Posts" },
  { path: "/about/", title: "About" },
  { path: "/terms/", title: "Terms" },  // <-- add this
];
```

## Project structure

```
src/
  site.config.ts          ← Your name, bio, social links, site settings
  content/
    post/*.md             ← Blog posts (this is where you write)
    page/about.md         ← About page content
  data/showcase.ts        ← Showcase/portfolio entries
  pages/                  ← Routes (each .astro file = a page)
  layouts/                ← Page templates
  components/             ← Reusable UI pieces
  styles/global.css       ← Colors, fonts, design tokens
public/
  icon.png                ← Site favicon (512x512)
  social-card.png         ← Default social preview image (1200x630)
  avatar.png              ← Your photo (optional)
```

## Common tasks

### Change site info or social links

Edit `src/site.config.ts` — your name, email, GitHub, LinkedIn, employer, etc. are all there.

### Change the About page

Edit `src/content/page/about.md`. It's plain Markdown.

### Replace site images

Drop your files into `public/`:
- `icon.png` (512x512) — favicon and PWA icon
- `social-card.png` (1200x630) — default image shown when sharing links on social media
- `avatar.png` — your photo

### Change colors or fonts

Edit `src/styles/global.css`. Light and dark theme colors are at the top of the file under `[data-theme="light"]` and `[data-theme="dark"]`.

### Hide the showcase section

The showcase section is already empty. If you want to add projects to it later, edit `src/data/showcase.ts`.

## Deploying

`pnpm build` produces a static `dist/` folder. Upload it to any static host: Netlify, Vercel, Cloudflare Pages, GitHub Pages, or a simple file server.

## Theme updates

To pull improvements from the upstream theme:

```sh
git remote add theme https://github.com/AnjayGoel/astro-sienna.git
git fetch theme
git merge theme/main --allow-unrelated-histories
```
