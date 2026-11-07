# CLAUDE.md

Ce fichier guide Claude (et tout contributeur) lorsqu'il travaille dans ce dépôt.

## Le projet

Site personnel / CV en ligne d'Emmanuel Enkirche, publié sur GitHub Pages et servi
sous le domaine personnalisé `www.enkirche.fr` (fichier `CNAME`).

C'est un fork du thème Jekyll **{ Personal }** (le4ker/personal-jekyll-theme, v10.x).
Le `README.md` et le `CHANGELOG.md` sont ceux du thème amont, pas du site.

Langue du site : **français** (`lang: "fr"`). Tout contenu visible doit être rédigé
en français.

## Stack

- Jekyll 4.2.2, Ruby 3.1.1 (`.ruby-version`)
- Plugins : `jekyll-paginate`, `jekyll-spaceship` (Mermaid, tableaux enrichis)
- Bootstrap + SCSS (`_sass/`, `css/`), coloration syntaxique `everforest`
- Commentaires Cusdis (`cusdis-app-id` dans `_config.yml`)

## Commandes

```shell
# Docker (recommandé, aucune install Ruby locale)
docker-compose up --build          # http://localhost:4000

# Ruby natif
bundle install
bundle exec jekyll serve --watch --livereload
bundle exec jekyll build           # génère _site/

# Nouvel article (crée _posts/AAAA-MM-JJ-<slug>.md avec le front matter)
ruby bin/new_post.rb <slug>

# Pages de tags / catégories manquantes (à lancer après ajout d'articles)
ruby bin/generate_tags.rb
ruby bin/generate_categories.rb
```

Il n'y a pas de suite de tests. La vérification consiste à lancer `jekyll build`
sans erreur, puis à relire la page générée.

## Où se trouve quoi

| Élément à modifier                                   | Fichier                                       |
| ---------------------------------------------------- | --------------------------------------------- |
| Titre, description, citation d'accueil, portrait     | `_config.yml` (`title`, `description`, `quote`, `me-img`) |
| Parcours professionnel (frise chronologique)         | `_config.yml` → `events` (du plus récent au plus ancien) |
| Texte de présentation sous les articles et « About » | `_config.yml` → `author_blurb`                |
| Réseaux sociaux                                      | `_config.yml` → `social`                      |
| Sections affichées sur l'accueil                     | `_config.yml` → `pages_list`                  |
| Gabarits HTML des sections                           | `_includes/` (about, timeline, contact…)      |
| Mise en page                                         | `_layouts/`                                   |
| Styles, thème clair/sombre                           | `_sass/`, `css/grayscale.scss`                |
| Articles de blog                                     | `_posts/`                                     |
| Images de la frise                                   | `img/timeline/`                               |

Front matter attendu pour un article :

```yaml
---
layout: post
section-type: post
has-comments: true
title: Titre
category: categorie
tags: ["tag1", "tag2"]
---
```

Chaque catégorie et chaque tag doit avoir sa page dans `categories/` et `tags/`
(créée par les scripts `bin/`), sinon le lien est cassé.

## Déploiement et branches

- Branche par défaut : **`master`**. Un push sur `master` déclenche
  `.github/workflows/jekyll.yml`, qui construit et **publie immédiatement** le site.
- `ci.yml` (htmlproofer) et `release.yml` (semantic-release) ciblent la branche
  `main`, héritée du thème : ils ne se déclenchent donc jamais ici.
- Messages de commit au format Conventional Commits (`feat:`, `fix:`, `docs:`…).
  Le hook se configure avec `sh .githooks/setup.sh`.

## Points d'attention connus

- Tout fichier sans front matter non listé dans `exclude` (`_config.yml`) est
  copié tel quel dans le site publié. Ajouter à `exclude` tout nouveau fichier
  interne (notes, docs, scripts).
- `url` vaut `https://manubarnum.github.io` alors que le site est servi sur
  `www.enkirche.fr` : à garder en tête pour les URL absolues, le flux RSS et le
  sitemap.
- `_posts/` contient encore les articles tutoriels du thème (2015, en anglais).
- `_includes/about.html` affiche encore les boutons « star / fork » du dépôt du
  thème et une phrase en anglais.
- `timeline-img` pointe vers `img/timeline/defaut.png` ; `default.png` existe
  aussi. Ne pas supprimer l'un sans vérifier `_config.yml`.
- L'adresse `email` du contact est une adresse jetable : la changer seulement à
  la demande du propriétaire.
- Les images de démo du thème (`*tocat*`, `intro-bg.jpg`) sont inutilisées par le
  contenu actuel, mais vérifier les gabarits avant de les retirer.
