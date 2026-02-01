# pratiksethi.dev

Personal blog built with [Hugo](https://gohugo.io/) and the [Coder](https://github.com/luizdepra/hugo-coder) theme.

## Development

### Prerequisites

- [Hugo](https://gohugo.io/installation/) (extended version)

### Local Development

```bash
# Start dev server with drafts visible
hugo server -D

# Site available at http://localhost:1313
```

### Create New Post

```bash
hugo new posts/my-new-post.md
```

Posts are created as drafts by default. Set `draft: false` in frontmatter to publish.

### Build

```bash
hugo --minify
```

Output goes to `public/` directory.

## Deployment

Pushes to `main` branch automatically deploy to GitHub Pages via GitHub Actions.

## Content Structure

- `content/posts/` - Blog posts
- `static/` - Static files (images, CNAME, etc.)
- `hugo.toml` - Site configuration
