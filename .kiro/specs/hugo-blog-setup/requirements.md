# Requirements Document

## Introduction

This document specifies the requirements for setting up a personal blog at pratiksethi.dev using Hugo static site generator with the Coder theme. The blog will be hosted on the existing GitHub Pages infrastructure and will migrate existing markdown content from the `blog/` folder to Hugo's content structure.

## Glossary

- **Hugo**: A fast static site generator written in Go that builds websites from markdown content and templates
- **Coder_Theme**: A minimal Hugo theme designed for developers, featuring clean typography and code highlighting
- **GitHub_Pages**: A static site hosting service that serves content directly from a GitHub repository
- **Frontmatter**: YAML metadata at the top of markdown files that defines properties like title, date, and tags
- **Content_Section**: A top-level directory in Hugo's content folder that groups related content types
- **Base_URL**: The root URL where the site is hosted (pratiksethi.dev)
- **Build_System**: The Hugo build process that generates static HTML from markdown and templates

## Requirements

### Requirement 1: Hugo Project Initialization

**User Story:** As a blog owner, I want a properly configured Hugo project, so that I can build and deploy my blog using Hugo's static site generation.

#### Acceptance Criteria

1. THE Build_System SHALL generate a valid Hugo project structure with config file, content directory, and themes directory
2. WHEN Hugo builds the site, THE Build_System SHALL produce static HTML files in the public directory
3. THE Build_System SHALL configure the Base_URL as "https://pratiksethi.dev/"
4. WHEN the site is built, THE Build_System SHALL generate valid HTML that renders correctly in modern browsers

### Requirement 2: Coder Theme Integration

**User Story:** As a blog owner, I want the Coder theme installed and configured, so that my blog has a clean, developer-focused design.

#### Acceptance Criteria

1. THE Build_System SHALL include the Coder_Theme as a Git submodule in the themes directory
2. WHEN the site is built, THE Coder_Theme SHALL render the site with proper navigation, typography, and code highlighting
3. THE Coder_Theme SHALL be configured with site author information (Pratik Sethi)
4. THE Coder_Theme SHALL display social links for GitHub and LinkedIn profiles
5. WHEN viewing on mobile devices, THE Coder_Theme SHALL render a responsive layout

### Requirement 3: Blog Post URL Structure

**User Story:** As a blog reader, I want clean URLs for blog posts, so that I can easily share and bookmark content.

#### Acceptance Criteria

1. WHEN a blog post is published, THE Build_System SHALL generate URLs in the format `pratiksethi.dev/posts/<slug>/`
2. THE Build_System SHALL derive the URL slug from the post's filename or frontmatter slug field
3. WHEN a post has a custom slug in frontmatter, THE Build_System SHALL use that slug instead of the filename
4. THE Build_System SHALL NOT include date components in the URL path

### Requirement 4: Content Migration

**User Story:** As a blog owner, I want my existing blog posts migrated to Hugo format, so that I preserve my existing content.

#### Acceptance Criteria

1. WHEN migrating existing posts, THE Build_System SHALL preserve all Frontmatter fields (title, description, date, tags, draft)
2. THE Build_System SHALL place migrated posts in the `content/posts/` directory
3. WHEN a post has `draft: true` in Frontmatter, THE Build_System SHALL exclude it from production builds by default
4. THE Build_System SHALL preserve the original markdown content without modification

### Requirement 5: Content Organization and Tag Filtering

**User Story:** As a blog owner, I want organized content sections with tag-based filtering, so that I can publish different types of content (tutorials, learnings, personal musings) and readers can find related posts.

#### Acceptance Criteria

1. THE Build_System SHALL support a posts Content_Section for all blog content
2. WHEN displaying posts, THE Build_System SHALL show posts sorted by date in descending order (newest first)
3. THE Build_System SHALL enable Hugo's tags taxonomy for categorizing posts
4. WHEN a tag is clicked, THE Build_System SHALL display a dedicated tag page listing all posts with that tag
5. THE Build_System SHALL generate a tags index page at `/tags/` showing all available tags
6. WHEN viewing a post, THE Coder_Theme SHALL display the post's tags as clickable links

### Requirement 6: Homepage Configuration

**User Story:** As a blog visitor, I want a welcoming homepage, so that I understand who owns the blog and can navigate to content.

#### Acceptance Criteria

1. WHEN visiting the Base_URL, THE Coder_Theme SHALL display a homepage with author introduction
2. THE Coder_Theme SHALL display navigation links to the posts section
3. THE Coder_Theme SHALL display recent posts or a link to all posts from the homepage
4. THE Coder_Theme SHALL display the site title "Pratik Sethi" prominently

### Requirement 7: GitHub Pages Deployment

**User Story:** As a blog owner, I want automated deployment to GitHub Pages, so that my blog updates when I push changes.

#### Acceptance Criteria

1. THE Build_System SHALL include a GitHub Actions workflow for building and deploying the site
2. WHEN changes are pushed to the main branch, THE Build_System SHALL trigger an automated build and deployment
3. THE Build_System SHALL deploy the generated static files to GitHub Pages
4. THE Build_System SHALL preserve the CNAME file for custom domain configuration
5. IF the Hugo build fails, THEN THE Build_System SHALL report the error and not deploy broken content

### Requirement 8: Development Workflow

**User Story:** As a blog author, I want a local development server, so that I can preview changes before publishing.

#### Acceptance Criteria

1. WHEN running Hugo's development server, THE Build_System SHALL serve the site locally with live reload
2. WHEN running in development mode, THE Build_System SHALL include draft posts in the preview
3. THE Build_System SHALL provide clear documentation for common development commands

### Requirement 9: Flexible Content Formats

**User Story:** As a blog author, I want to write content in different formats, so that I can choose the best format for each piece of content.

#### Acceptance Criteria

1. THE Build_System SHALL support markdown (.md) files as the primary content format
2. THE Build_System SHALL support HTML files for custom pages that need full HTML control
3. WHEN an HTML file is placed in the content directory with proper frontmatter, THE Build_System SHALL process it as content
4. THE Build_System SHALL support custom JavaScript in pages through Hugo's asset pipeline or static files
5. WHEN creating interactive pages, THE Build_System SHALL allow embedding JavaScript within HTML content files
