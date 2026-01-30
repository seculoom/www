
# Security and Privacy Laboratory

## Install

```bash
uv venv --python 3.11
source venv/bin/activate
pip install -r requirements.txt
```

## mkdocs commands

```bash
mkdocs new [dir-name] # Create a new project.
mkdocs serve          # Start the live-reloading docs server.
mkdocs build          # Build the documentation site.
mkdocs -h             # Print help message and exit.
```

## Folder layout

- mkdocs.yml：設定ファイル
- docs/：ドキュメントのルートフォルダ

```plaintext
docs
├── contact.md
├── index.md
├── lectures.md
├── people
│   └── akira_yamada.md
├── people.md
├── privacy-policy.md
├── projects.md
├── publications.md
├── research.md
├── stylesheets
│   └── extra.css
└── tools.md
```
## 公開までの流れ

1. GitHub上での設定
    - リポジトリのSettings > Pagesで、Sourceを`GitHub Actions`に設定
2. ローカルでの操作
    - .github/workflows/pages.ymlを作成

```yaml
name: Deploy GitHub Pages

on:
  push:
    branches:
      - main

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Build
        run: |
          # 例:
          # pip install mkdocs
          # mkdocs build
          echo "build step"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./site   

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

3. mainブランチにpushすると、自動的にGitHub Pagesにデプロイされる
