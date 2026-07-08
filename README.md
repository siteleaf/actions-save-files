# siteleaf/actions-save-files
Action to save files from GitHub Actions to Siteleaf Preview

Example usage:

```yaml
# .github/workflows/preview.yml

name: Build preview with Jekyll and Sitleaf

on:
  # Runs on pushes targeting the default branch(s)
  push:
    branches: ["**"]

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
          bundler-cache: true
          
      - name: Build Jekyll site    # Replace with your custom build steps
        run: bundle exec jekyll build

      - name: Deploy to Siteleaf
        uses: siteleaf/actions-save-files@v1
        with:
          publish_dir: _site       # Optional (defaults to _site)
          compare_size_only: true  # Optional (faster)

```
