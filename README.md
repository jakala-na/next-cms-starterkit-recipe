# Next CMS Starter Drupal + GraphQL example site recipe

# Use and installation

## Create a Drupal recommended project

```
mkdir next-cms-starterkit-drupal && cd next-cms-starterkit-drupal
ddev config --project-type=drupal --php-version=8.3 --docroot=web
ddev start
ddev composer create drupal/recommended-project:^10
ddev config --update
ddev composer require drush/drush
ddev drush site:install minimal --account-name=admin --account-pass=admin -y
```

## Configure composer.json

Allow `dev` stability composer projects to be installed:

```
ddev composer config minimum-stability --merge --json "dev"
```

## Ignoring downloaded recipes

```
echo "/recipes" >> .gitignore
```

## Require the recipe

```
composer config repositories.next-cms-starterkit-recipe  vcs https://github.com/jakala-na/next-cms-starterkit-recipe
ddev composer require jakala-na/next-cms-starterkit-recipe
```

## Apply the recipe

```
ddev exec -d /var/www/html/web php core/scripts/drupal recipe ../recipes/next-cms-starterkit-recipe
```

# What this recipe does:

Currently, nothing :)
