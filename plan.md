# Plan: A Feature-Rich Academic Webpage with MkDocs Material

## Core Idea
This plan outlines the steps to create a professional academic website that is both easy to manage and rich with features. We will build a site that not only showcases your CV and publications but also integrates BibTeX for automated citation management, displays research visuals, and has a custom "warm and welcoming" design.

## Phase 1: Project Setup & Configuration

This phase is about setting up the project with all the necessary plugins.

### 1. Install Required Packages
First, make sure you have Python installed. Then, open a terminal and run:
```bash
pip install mkdocs mkdocs-material mkdocs-bibtex mkdocs-git-revision-date-localized-plugin
```

### 2. Initialize Your Project
Navigate to your `personal-page` directory and initialize the MkDocs project.
```bash
cd C:\GitHub\personal-page
mkdocs new .
```

### 3. Create the Directory Structure
This structure will keep your content, references, and custom styles organized.
```
personal-page/
├── docs/
│   ├── index.md
│   ├── publications.md
│   ├── cv.md
│   ├── references.bib    # Your BibTeX file
│   └── assets/
│       ├── images/       # Logos, profile picture, research visuals
│       └── stylesheets/
│           └── extra.css # Custom styles
└── mkdocs.yml
```

### 4. Configure `mkdocs.yml`
Replace the contents of `mkdocs.yml` with this configuration. It enables the light/dark mode toggle, search, BibTeX integration, and revision dates.

```yaml
site_name: "Dr. Job Schepens"
site_author: "Dr. Job Schepens"
site_url: "https://jobschepens.github.io/personal-page/"

theme:
  name: material
  language: en
  custom_dir: docs/overrides
  palette:
    - media: "(prefers-color-scheme: light)"
      scheme: default
      primary: brown
      accent: amber
      toggle:
        icon: material/weather-night
        name: Switch to dark mode
    - media: "(prefers-color-scheme: dark)"
      scheme: slate
      primary: blue
      accent: light-blue
      toggle:
        icon: material/weather-sunny
        name: Switch to light mode
  features:
    - navigation.tabs
    - navigation.top
    - content.code.copy
    - search.suggest
    - search.highlight

extra:
  social:
    - icon: fontawesome/brands/google-scholar
      link: https://scholar.google.de/citations?user=4S18kYgAAAAJ&hl
    - icon: fontawesome/brands/orcid
      link: https://orcid.org/0000-0003-1271-2526
    - icon: fontawesome/brands/linkedin
      link: https://www.linkedin.com/in/job-schepens/
    - icon: fontawesome/brands/github
      link: https://github.com/jobschepens

nav:
  - Home: index.md
  - Publications: publications.md
  - CV: cv.md

plugins:
  - search
  - git-revision-date-localized:
      type: timeago
  - bibtex:
      bib_file: docs/references.bib

markdown_extensions:
  - admonition
  - attr_list
  - md_in_html
  - pymdownx.details
  - pymdownx.superfences
  - footnotes
```

## Phase 2: Content, Visuals, and Styling

### 1. Create Your BibTeX File
- Create a file named `references.bib` inside the `docs/` directory.
- Add all your publications to this file in BibTeX format. You can export these from Google Scholar or your reference manager.
  ```bibtex
  @article{schepens2020big,
    title={Big data suggest strong constraints of linguistic similarity on adult language learning},
    author={Schepens, Job and van Hout, Roeland and Jaeger, T Florian},
    journal={Cognition},
    volume={194},
    pages={104056},
    year={2020},
    publisher={Elsevier}
  }
  ```

### 2. Create the Publications Page (`docs/publications.md`)
This page will automatically display your publications from `references.bib`. You can categorize and filter them.

- **Add Research Visuals**: For key publications, you can add images or figures directly within the publication entry. Place the images in `docs/assets/images`.

- Edit `docs/publications.md` to look like this:

  ```markdown
  # Publications

  ## Journal Articles
  Here are my peer-reviewed journal articles.

  ::: publications
      [type=article]
  ---
  
  ### Key Publication Highlight
  
  **Big data suggest strong constraints of linguistic similarity on adult language learning**
  
  ![Research Visual](assets/images/cognition2020-figure.png){: style="width:400px;"}
  
  This paper demonstrates the powerful effect of language distance on learning outcomes. The visual above shows the core finding.
  
  ---

  ## Conference Proceedings
  
  ::: publications
      [type=inproceedings]
  ```

### 3. Create Your Homepage and CV
- **Homepage (`docs/index.md`)**: Create a welcoming homepage with a brief bio, your research interests, and logos of your university and funders.
- **CV Page (`docs/cv.md`)**: Copy the content from your `schepens-cv-250914.md` file into `docs/cv.md`.

### 4. Add "Warm and Welcoming" Custom CSS
- Create a file named `extra.css` in `docs/assets/stylesheets/`.
- Add the following CSS to create a warmer look. This example uses an off-white background and a gentle color palette.

  ```css
  /* Custom warm and welcoming theme */
  :root {
    --md-primary-fg-color:        #5D4037; /* A warm brown */
    --md-accent-fg-color:         #FFA000; /* A gentle amber */
  }

  [data-md-color-scheme="default"] {
    --md-default-fg-color: #333;
    --md-default-bg-color: #fdfdfd; /* Slightly off-white background */
  }
  ```
- To apply these styles, add this to your `mkdocs.yml`:
  ```yaml
  extra_css:
    - assets/stylesheets/extra.css
  ```

### 5. Preview Your Site
Run `mkdocs serve` and open `http://127.0.0.1:8000` to see your site.

## Phase 3: Automated Deployment with GitHub Actions

Instead of deploying manually, we'll set up a GitHub Action to do it for you automatically.

### 1. Create the Workflow File
- Create a directory path `.github/workflows/` in the root of your `personal-page` project.
- Inside that directory, create a file named `ci.yml` and add the following content:

  ```yaml
  name: Build and Deploy
  on:
    push:
      branches:
        - main
  jobs:
    deploy:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v3
        - name: Set up Python
          uses: actions/setup-python@v4
          with:
            python-version: 3.x
        - name: Install dependencies
          run: pip install mkdocs-material mkdocs-bibtex mkdocs-git-revision-date-localized-plugin
        - name: Deploy
          run: mkdocs gh-deploy --force
  ```

### 2. How It Works
- Now, every time you `git push` your changes to the `main` branch, this action will automatically run.
- It will build your MkDocs site and deploy it to the `gh-pages` branch.
- Your live site will be updated within a few minutes.

This revised plan gives you a powerful, automated, and professional academic website that meets all your requirements.
