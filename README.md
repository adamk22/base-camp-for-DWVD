## About Base Camp x DWVD

> A customized fork of the Base Camp starter theme.

## TODO

-   Refactor admin code in config
-   Alternative dist folder with prod/dev flag/environments (webpack, eg: dist-dev)
-   Remove or refactor the purgeCss webpack module?

A gentle warning. This fork has undocumented changes. Use at own risk. Will document it better when I have the time :-)

## Customizations

-   Replaced Bulma with Boostrap 4
-   Replaced SASS with SCSS and added a complete ITCSS structure
-   Removed WooCommerce boilerplate code
-   Removed some other boilerplate code **we** don't need
-   Added Javascript workflow based on routes (based from Sage 9.0)
-   Added 2 additional files in `/config` to clean up the admin panel and config roles

## Installation (Theme Only)

-   Go your themes folder and run`git clone https://github.com/adamk22/base-camp.git theme-name`
-   `cd theme-name`
-   `yarn` **or** `npm install`
-   `composer install`
-   define your custom webpack config to `build/config.js` file
-   create a .env file based on the .env.example (or just rename it to .env)
-   `yarn watch` **or** `npm run watch`
-   Happy developing :)

## Installation (Using Bedrock provisioning file)

-   Follow the instructions that comes with the [provisioning file](https://github.com/adamk22/WP-Bedrock-VVV-2.0-Provisioning).
-   The provisioning file will install the theme, run composer and npm.
-   define your custom webpack config to `build/config.js` file
-   create a .env file based on the .env.example
-   `yarn watch` **or** `npm run watch`
-   Happy developing :)

## Structure

```
base-camp/                                          # Theme root
├── app/                                            # Theme logic
│   ├── config/                                     # Theme config
│   │   ├── wp/                                     # WordPress specific config
│   │   │   ├── admin-page.php                      # Register here WordPress Admin Page config
│   │   │   ├── image-sizes.php                     # Register here WordPress Custom image sizes
│   │   │   ├── login-page.php                      # Register here WordPress Login Page config
│   │   │   ├── maintenance.php                     # Maintenance mode config
│   │   │   ├── menus.php                           # Register here WordPress navigation menus
│   │   │   ├── scripts-and-styles.php              # Register here WordPress scripts and styles
│   │   │   ├── security.php                        # Things that increase the site security
│   │   │   ├── sidebars.php                        # Register here WordPress sidebars
│   │   │   └── theme-supports.php                  # Register here WordPress theme features
│   │   ├── autoload.php                            # Includes all config files (DON'T REMOVE THIS)
│   │   ├── timber.php                              # Timber specific config
│   │   └── woocommerce.php                         # Init woocommerce support
│   ├── models/                                     # Wrappers for Timber Classes
│   ├── timber-extends/                             # Extended Timber Classes
│   │   └── BaseCampSite.php                        # Extended TimberSite Class
│   ├── bootstrap.php                               # Bootstrap theme
│   ├── helpers.php                                 # Common helper functions
├── build/                                          # Theme assets and views
│   ├── config.js                                   # Custom webpack config
│   ├── webpack.config.js                           # Webpack config
├── resources/                                      # Theme assets and views
│   ├── assets/                                     # Front-end assets
│   │   ├── js/                                     # Javascripts
│   │   │   └── components/                         # Vue Components
│   │   ├── sass/                                   # Styles
│   │   │   └── components/                         # Partials
│   ├── languages/                                  # Language features
│   │   ├── base-camp.pot                           # Template for translation
│   │   └── messages.php                            # Language strings
│   ├── views/                                      # Theme Twig files
│   │   ├── components/                             # Partials
│   │   ├── footer/                                 # Theme footer templates
│   │   └── header/                                 # Theme header templates
```

## Models

> Models are wrapper classes for Wordpress Post Types and Taxonomies. They provide very simple interface to interact with the database.

### How to use

```
// index.php

<?php

use Basecamp\Models\Post;

// returns an array of TimberPost objects
Post::all();

// returns TimberPost object with the ID 1 (if it exists)
Post::find(1);

// returns first two posts;
Post::take(2)->get();

// skips first two posts;
Post::skip(2)->get();

// returns published posts;
// Supported values: https://codex.wordpress.org/Post_Status#Default_Statuses
Post::status('publish')->get();

// returns all posts except post with ID 1;
Post::exclude([1])->get();

// returns only posts with ID 1;
Post::include([1])->get();

// returns an array of posts descending order by author;
// Supported Values: https://codex.wordpress.org/Class_Reference/WP_Query#Order_.26_Orderby_Parameters
Post::orderby('author', 'desc')->get();

// returns an array of posts authored by admin;
Post::author('admin')->get();

// returns an array of posts which are in category 'cars' or 'vehicles';
Post::inCategory(['cars', 'vehicles'])->get();
```

> All queries are chainable. For example you can get three first incomplete posts authored by admin:

```
Post::status('draft')->author('admin')->take(3)->get();
```

All models are able to use almost every methods. However there are some exceptions:

-   Only `Post` model has `inCategory()` method
-   Taxonomies (Category, Tag) have `hideEmpty()` method

## Luna (Command-line interface)

> [Docs](https://github.com/suomato/luna)
