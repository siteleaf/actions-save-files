# siteleaf/actions-save-files
Action to save files from GitHub Actions to Siteleaf Preview

Example usage:

```yaml
# .github/workflows/preview.yml

name: Build preview with Jekyll and Sitleaf

on:
  push:
    branches:
      - main  # Build previews for main branch

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

permissions:
  contents: read
  id-token: write  # Required for siteleaf/actions-save-files
  
jobs:
  preview:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout branch
        uses: actions/checkout@v4
      
      - name: Setup Ruby           # Required only if using Jekyll
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: "3.2"      # Not needed with .ruby-version, .tool-versions, etc
          bundler-cache: true      # Runs 'bundle install' and caches installed gems
          
      - name: Build Jekyll site    # Replace with your custom build steps
        env:
          JEKYLL_ENV: staging
        run: |
          bundle exec jekyll build \
            --future \
            --unpublished \
            --drafts

      - name: Deploy to Siteleaf
        uses: siteleaf/actions-save-files@v1
        with:
          publish_dir: _site       # Optional (defaults to _site)
          compare_size_only: true  # Optional (faster)

```
