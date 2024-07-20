# Filament Issue

## Description

FilamentPHP when using `->path('/') or ->path('')` is causing a 404 error when routes are cached.

Apache `mod_alias` is being used to alias `/v2` to the FilamentPHP public
directory at `/var/www/html/public` accessed from https://localhost/v2

The current v1 public directory is at `/var/www/html/www.80` accessed from
https://localhost

This uses a Docker container with PHP 8.2 and apache to mimic the issue.

# Issue

When using `->path('/') or ->path('')` in the FilamentPHP routes, the following
error is thrown **ONLY** when the routes are cached:

```
The GET method is not supported for route /. Supported methods: HEAD.
```

You are still able to access any additional routes such as /v2/login, /v2/register, etc.
Though the root path `/v2` is causing the error.

If you clear the cache, `php artisan optimize:clear` the error goes away and the root path `/v2` functions as normal.

# Steps to Reproduce

1. Clone the repository
1. Install Composer packages
   `composer install`
1. Install node packages and build
   `npm install && npm run build`
1. Run the docker-compose file
   `docker compose up -d --build`
1. Add a the test user
   1. `php artisan filament:user`
   1. Enter name: `Test User`
   1. Enter email: `test@filamentphp.com`
   1. Enter password: `password`
1. You'll be able to access the site at
   1. v1 - http://localhost/
   1. Filament PHP - https://localhost/v2
1. Login to the Filament PHP site
   1. Go to https://localhost/v2/login
   1. Enter email: `test@filamentphp.com`
   1. Enter password: `password`
1. You'll be redirected to the dashboard
   1. https://localhost/v2/
1. Now cache the routes
   1. `php artisan route:cache`
1. Try to access the dashboard with the cached routes
    1. https://localhost/v2/
1. It will error with the following message
   `The GET method is not supported for route /. Supported methods: HEAD.`

# Expected Behavior

The root path `/v2` should not error when the routes are cached.



