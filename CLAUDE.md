# CLAUDE.md

Ce fichier guide Claude (et tout contributeur) lorsqu'il travaille dans ce dépôt.

## Le projet

Site personnel / CV en ligne d'Emmanuel Enkirche, publié sur GitHub Pages et servi
sous le domaine personnalisé `www.enkirche.fr` (fichier `CNAME`).

C'est un fork du thème Jekyll **{ Personal }** (le4ker/personal-jekyll-theme, v10.x),
traduit en français et débarrassé de ses contenus de démonstration.

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
| Texte de présentation (« À propos », bas d'article) | `_config.yml` → `author_blurb`                |
| Réseaux sociaux                                      | `_config.yml` → `social`                      |
| Sections affichées sur l'accueil et libellés du menu | `_config.yml` → `pages_list`                  |
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

- Branche unique : **`master`**. Un push sur `master` déclenche
  `.github/workflows/jekyll.yml`, qui construit et **publie immédiatement** le site.
- `ci.yml` construit le site et vérifie les liens internes (htmlproofer) sur les
  pull requests vers `master`.
- Messages de commit au format Conventional Commits (`feat:`, `fix:`, `docs:`…).
  Le hook se configure avec `sh .githooks/setup.sh`.

## Points d'attention

- Tout fichier sans front matter non listé dans `exclude` (`_config.yml`) est
  copié tel quel dans le site publié. Ajouter à `exclude` tout nouveau fichier
  interne (notes, docs, scripts).
- La clé `À propos` de `pages_list` est aussi lue par `_includes/header.html`
  (lien du portrait) : renommer les deux ensemble.
- `_posts/` est vide pour l'instant ; la section Blog est masquée dans
  `pages_list`. Les dossiers `tags/` et `categories/` ne contiennent qu'un
  `.gitkeep`, ignoré par les scripts `bin/`.
- `background-img` est défini dans `_config.yml` mais le thème actuel ne
  l'utilise pas : l'accueil a un fond uni.
- L'adresse `email` du contact est une adresse jetable : la changer seulement à
  la demande du propriétaire.
- Conserver les en-têtes de copyright du thème dans `_includes/` et le fichier
  `LICENSE` (Apache 2.0).
