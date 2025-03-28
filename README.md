# Next CMS Starter Drupal + GraphQL example site recipe

# Use and installation

## Install and configure Drupal

```
# Create Drupal recommended project
mkdir next-cms-starterkit-drupal && cd next-cms-starterkit-drupal
ddev config --project-type=drupal --php-version=8.3 --docroot=web
ddev start
ddev composer create drupal/recommended-project:^10
ddev config --update
ddev composer require drush/drush
ddev drush site:install minimal --account-name=admin --account-pass=admin -y

# Configure composer.json to allow dev stability packages
ddev composer config minimum-stability --merge --json "dev"

# Add the repository for the recipe to composer
ddev composer config repositories.next-cms-starterkit-recipe vcs https://github.com/jakala-na/next-cms-starterkit-recipe

# Git ignore downloaded recipes
echo "/recipes" >> .gitignore

# Require the recipe's composer dependencies
ddev composer require jakala-na/next-cms-starterkit-recipe

# Apply the recipe
ddev exec -d /var/www/html/web php core/scripts/drupal recipe ../recipes/next-cms-starterkit-recipe

# Generate consumers, including CLIENT_ID and CLIENT_SECRET values
ddev drush php:script recipes/next-cms-starterkit-recipe/scripts/consumers

# Rebuild Drupal permissions
ddev drush php:eval 'node_access_rebuild();'

# Rebuild Drupal cache
ddev drush cr

# Log into Drupal as admin
ddev drush uli
```

## Install and configure the next-cms-starterkit front-end

See https://github.com/jakala-na/next-cms-starterkit for starter kit instructions.

Add the generated `CLIENT_ID` and `CLIENT_SECRET` values to the `next-cms-starterkit/apps/drupal-marketing/.env.local` file:

```
DRUPAL_GRAPHQL_URI=http://next-cms-starterkit-drupal.ddev.site/graphql
DRUPAL_AUTH_URI=http://next-cms-starterkit-drupal.ddev.site/oauth/token
DRUPAL_PREVIEWER_CLIENT_ID=<previewer client id>
DRUPAL_PREVIEWER_CLIENT_SECRET=<previewer secret>
DRUPAL_VIEWER_CLIENT_ID=<viewer client id>
DRUPAL_VIEWER_CLIENT_SECRET=<viewer secret>
```

Start the front end:

```
turbo dev --filter=drupal-marketing
```

Go to http://localhost:3000/

# What this recipe does:

- Creates content types `page` and `article`
- Creates paragraph type `hero`
- Create media type `image`
- Creates default content
- Creates user roles `viewer` and `previewer`
- Creates user `editor`
- Configures languages English and Spanish
- Configures content translation
- Configures `pathauto`
- Configures `gin` as administration experience
- Configures GraphQL as the decoupling strategy
  - Configures GraphQL Compose
    - Edges
    - Image Styles
    - Menus
    - Routes
    - Users
    - Views
  - Configures preview
- Configures Simple OAuth as auth mechanism for the BE and the FE
- Configures Visual Editor for inline editorial experience
