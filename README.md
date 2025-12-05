# Larabox

> Author: Aaron Mangan
> Version: 0.1.0 [December 2025]
> Supports: PHP (>= 7.4) | Nginx | MySQL (8) | SQLite (?)

> [https://github.com/AaronMangan/larabox.git](https://github.com/AaronMangan/larabox.git)

## What is larabox?

Larabox is an project that uses Docker to serve Laravel projects (or other PHP projects if you want)

## Doesn't LaraDock do that?

Yep, and it has EVERYTHING - if you need something that isn't the default service I highly recommend it!

## So: why Larabox?

Well, I wanted something that just had what I needed - all the Laravel apps that I used have been the standard Nginx, MySQL, PHP and LaraDock comes with a lot of extras. Another reason is that, in Laradock, you need a separate nginx conf file for each app - even if they're identical, except the server name.

## Wait, so you dont need that here?

Nope, with Larabox all Laravel apps in the configured directory are served using the folder name as the TLD!

If you're old enough to remember Laravel Valet, think of it like Valet & Sail got freaky.

## How does that work then?

Lets say you have a folder setup like:

```
- repos
  - larabox
  - apps
    - laravel-one
    - laravel-two
```

then, in apps:

```
- apps
  - laravel-one
    - app
    - bootstrap
    - config
    - database
    - etc...
    - public
  - laravel-two
    - app
    - bootstrap
    - config
    - database
    - etc...
    - public
```
when you run larabox, you can go to the browser and enter `http://laravel-one.localhost` and see the `laravel-one` project, `http://laravel-two.localhost` will go to the `laravel-two` project. You may still need to run `npm run dev` in each one though but you dont have to stop and start containers between projects.

This is especially usefully if you have more than one project that needs to talk to each other, or you work in a consultancy or a team with a few similar projects.

It's also just a great starting place for your own custom Docker setup, or if you have junior developers that you just want it to work for.
___

# Before Starting

Larabox can really be installed where, though I recommend the setup pictured above which allows you to keep all your Laravel projects in one place and ready to serve.

You may of course do whatever you like but remember to update the `docker-compose.yml` file if you need to have multiple directories. Just make sure all your projects are sent to the `/var/www/` folder.

```
  - var
    - www
      - laravel-one
      - laravel-two
```

# Installation

To install Larabox, clone the repository with `git clone https://github.com/AaronMangan/larabox.git`

# Starting

There are a few way to start larabox, if you want every service try:

```shell
    cd larabox
    docker-compose up -d --build
```

otherwise, if you arent using mysql, try:

```shell
    cd larabox
    docker-compose up -d --build nginx workspace
```
or

```shell
    cd larabox
    docker-compose up -d --build nginx workspace mailpit
```

# Using Larabox

**If you are familiar with Docker than it's all the same here**

Access a container with: `docker exec -it <container-name> /bin/bash`:

```yaml
  - docker exec -it workspace /bin/bash                     # Run artisan commands here.
  - docker exec -it mysql mysql -u<configured-user>         # To access the MySQL database.
  - docker exec -it nginx /bin/bash                         # For nginx container access.
```

> TODO: I want to make a script that will automate these. Like `larabox workspace` or `larabox mysql`

# Adding Services

Services can be added just like any regular Docker setup, just edit the `docker-compose.yml` file with what you need.

## Pro Tip

> You may need to add the following block to your `vite.config.js` file:

```js
    server: { 
        hmr: {
            host: 'localhost',
        },
    },
```

And occasionally, I need to run `npm run dev` and then delete the `hot` file in the public directory.