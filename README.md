# enkirche.fr

Site personnel et CV en ligne d'Emmanuel Enkirche, publié sur GitHub Pages à
l'adresse [www.enkirche.fr](https://www.enkirche.fr).

## Lancer le site en local

Avec Docker (aucune installation Ruby nécessaire) :

```shell
docker-compose up --build
```

Ou avec Ruby 3.1 :

```shell
bundle install
bundle exec jekyll serve --watch --livereload
```

Le site est alors disponible sur `http://localhost:4000`.

## Modifier le contenu

Presque tout se règle dans `_config.yml` :

- `quote`, `me-img` : texte et portrait de l'accueil ;
- `author_blurb` : présentation (section « À propos » et bas des articles) ;
- `events` : parcours professionnel, du plus récent au plus ancien ;
- `social` : liens LinkedIn, GitHub, RSS ;
- `email` : adresse affichée dans la section « Contact ».

Pour écrire un article :

```shell
ruby bin/new_post.rb mon-article
ruby bin/generate_tags.rb && ruby bin/generate_categories.rb
```

## Publication

Chaque push sur la branche `master` reconstruit et publie le site
automatiquement (workflow `.github/workflows/jekyll.yml`).

## Crédits

Basé sur le thème Jekyll
[{ Personal }](https://github.com/le4ker/personal-jekyll-theme) de Panos Sakkos,
sous licence Apache 2.0 (voir [LICENSE](LICENSE)).
