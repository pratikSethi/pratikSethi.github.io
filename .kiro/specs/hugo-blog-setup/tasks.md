# Implementation Plan: Hugo Blog Setup

## Overview

This plan sets up a Hugo blog at pratiksethi.dev with the Coder theme, migrates existing content, and configures GitHub Actions for automated deployment. Tasks are ordered to enable incremental testing—each step produces a working state.

## Tasks

- [x] 1. Initialize Hugo project structure
  - Run `hugo new site . --force` to create Hugo structure in existing repo
  - Create `hugo.toml` with baseURL, title, and permalink configuration
  - Configure permalinks: `posts = "/posts/:slug/"`
  - Enable tags taxonomy
  - _Requirements: 1.1, 1.3, 3.1, 3.4, 5.3_

- [x] 2. Add Coder theme as Git submodule
  - Run `git submodule add https://github.com/luizdepra/hugo-coder.git themes/hugo-coder`
  - Set `theme = "hugo-coder"` in hugo.toml
  - Configure theme params: author, description, social links (GitHub, LinkedIn)
  - Add navigation menu items for Posts and Tags
  - _Requirements: 2.1, 2.3, 2.4, 6.2_

- [x] 3. Migrate existing blog content
  - Create `content/posts/` directory
  - Copy `blog/why-this-blog-exists.md` to `content/posts/hello-world.md`
  - Copy `blog/001-how-i-built-this-site.md` to `content/posts/how-i-built-this-site.md`
  - Verify frontmatter is preserved (title, description, date, tags, draft)
  - _Requirements: 4.1, 4.2, 4.3, 4.4_

- [x] 4. Configure static files and CNAME
  - Create `static/` directory
  - Copy `CNAME` file to `static/CNAME` (Hugo copies static/ contents to public/)
  - _Requirements: 7.4_

- [x] 5. Checkpoint - Verify local build
  - Run `hugo --minify` and verify public/ directory is created
  - Verify posts appear at public/posts/<slug>/index.html
  - Verify CNAME is in public/
  - Verify draft post is excluded (001-how-i-built-this-site.md has draft: true)
  - Run `hugo server -D` to preview with drafts
  - _Requirements: 1.2, 3.1, 4.3, 7.4, 8.2_

- [x] 6. Create GitHub Actions deployment workflow
  - Create `.github/workflows/deploy.yml`
  - Configure workflow to trigger on push to main branch
  - Use peaceiris/actions-hugo for Hugo setup
  - Use actions/deploy-pages for GitHub Pages deployment
  - Include submodule checkout (for theme)
  - _Requirements: 7.1, 7.2, 7.3, 7.5_

- [x] 7. Create archetype template for new posts
  - Create `archetypes/default.md` with standard frontmatter template
  - Include: title, description, date, tags, draft fields
  - _Requirements: 4.1_

- [x] 8. Update README with development instructions
  - Document local development commands: `hugo server -D`
  - Document build command: `hugo --minify`
  - Document how to create new posts: `hugo new posts/my-post.md`
  - _Requirements: 8.3_

- [x] 9. Clean up old files
  - Remove `index.html` (Hugo generates homepage)
  - Remove `blog/` directory (content migrated to content/posts/)
  - Keep `.gitignore`, update if needed to ignore public/
  - _Requirements: 1.1_

- [x] 10. Final checkpoint - Verify complete setup
  - Run `hugo --minify` and verify all output
  - Verify URL structure: /posts/<slug>/
  - Verify tags pages generated at /tags/
  - Verify homepage renders with Coder theme
  - Commit all changes
  - _Requirements: 1.2, 1.4, 3.1, 5.4, 5.5, 6.1_

## Notes

- Hugo must be installed locally (v0.110+ recommended for extended features)
- After pushing to main, GitHub Actions will deploy automatically
- Draft posts (draft: true) won't appear on production site
- Use `hugo server -D` locally to preview drafts
