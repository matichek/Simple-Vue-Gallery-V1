# Gallery with Vue/Nuxt/Vuetify/Axios

Why Nuxt.js - to simplify routing, passing SEO info

WIP (0_1)

TODO: 
- (DONE) Users screen - scolling grid - avaliable users on the system - each cell - Username, name and random photo from their album
- (DONE) Album screen - scrolling list of available albums - Album title and some random photo from the album
- (DONE) Photo screen - scrolling grid list of photos - titled overlayed
- (DONE) Photo screen - single photo gallery, view and close button

IMPROVEMENTS TODO:
- Title of the image, album title, name of the album user, details view can be shown or hidden by clicking on the image- (title of the image, album title, name of the user who created it.)
- Error, api calls components

Static deployment [DEMO](https://concepts.2gika.si/gal/)

## Build Setup

```bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm run start

# generate static project
$ npm run generate
```

For detailed explanation on how things work, check out the [documentation](https://nuxtjs.org).

## Special Directories

You can create the following extra directories, some of which have special behaviors. Only `pages` is required; you can delete them if you don't want to use their functionality.

### `assets`

The assets directory contains your uncompiled assets such as Stylus or Sass files, images, or fonts.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/assets).

### `components`

The components directory contains your Vue.js components. Components make up the different parts of your page and can be reused and imported into your pages, layouts and even other components.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/components).

### `layouts`

Layouts are a great help when you want to change the look and feel of your Nuxt app, whether you want to include a sidebar or have distinct layouts for mobile and desktop.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/layouts).


### `pages`

This directory contains your application views and routes. Nuxt will read all the `*.vue` files inside this directory and setup Vue Router automatically.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/get-started/routing).

### `plugins`

The plugins directory contains JavaScript plugins that you want to run before instantiating the root Vue.js Application. This is the place to add Vue plugins and to inject functions or constants. Every time you need to use `Vue.use()`, you should create a file in `plugins/` and add its path to plugins in `nuxt.config.js`.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/plugins).

### `static`

This directory contains your static files. Each file inside this directory is mapped to `/`.

Example: `/static/robots.txt` is mapped as `/robots.txt`.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/static).

### `store`

This directory contains your Vuex store files. Creating a file in this directory automatically activates Vuex.

More information about the usage of this directory in [the documentation](https://nuxtjs.org/docs/2.x/directory-structure/store).


It uses public testing API for the calls.

### Getting all images for the album:

https://jsonplaceholder.typicode.com/albums/1/photos

### Getting all users:

https://jsonplaceholder.typicode.com/users/

### Getting all albums:

https://jsonplaceholder.typicode.com/albums/
