## CRUD
#### for Laravel
by webbtj  

I promise I'll make a better readme before I publish this.  

### Purpose?
It's a package to automatically add crud routes/controller functionality
without the need to manually create views, controller, etc. Why? I had some
free time at work.  

So with this, you can just install the package, publish the config file, create
basic (blank) models (`php artisan make:model Car` for example), create/run
migrations to build the schema for your models and add the class names of each
model you want crud for to the config file. Bam! It's really just that quick
and easy.

You can add validation and some other stuff like readonly/inclusion/exclusion
declarations in the config as well. You should be able to get simple crud up and
running in a few minutes. ~Right now it just uses the `web` middleware, but I'll
add the ability to add your own middleware, and add api middleware automatically
in the future.~ *You can now inject middleware to the web and api routes, and
the api routes are there automatically. (There might not be proper json error
responses yet)* Probably a good idea to take a look through the config file that
gets published to get an idea on how that works.

Oh, and the properties are all read from the db schema the sameish way
Eloquent does. Also, you can publish the views if you like and mess around with
those. You also can create copies of any views in your `views` directory under
singular-downcase version of the model name (example `car`) and it will pick
your custom ones first. Of course when you publish the views it will put them
all under `crud`, those are the generic ones that apply to all models when they
don't have a custom version of the given view.  

### Update
You can now generate crud views, controllers, and api controllers with a
command. `php artisan crud:publish` will (without options) create crud views,
crud web and api controllers, suggest web and api route resources to add, and
recommend the removal of configs from the crud config file (so you're not
redeclaring the same routes and stuff). You can pass `--model` to specify which
model(s) you want to create components (controllers, views) for. You can pass
multiple (example: `--model=Car --model=House`). The model(s) do need to be
declared in your crud config or they'll be ignored. You can (and probably
should) use the full namespace (`--model=App\Car`) but if everything is in the
same space, you'll be ok to omit it. It's also case insensitive. You can also
pass `--type` to specify the thing(s) you want to publish, controllers, api
controllers, and/or views. Again, you can provide multiple options, but note
that the `type` and the `model` options are processed together, so each `type`
option will apply to all of the provided models. Here are the type options:
* `controller` - this will create a web controller under `Http/Controllers`.
* `api.controller` - this will create an API controller under
`Http/Controllers/Api`.
* `view` - this will create all crud views, as well as the main crud layout
parent template.
* `view.create`, `view.show`, `view.edit`, `view.index` - this will create the
respective crud templates.  

_Note regarding pluralization and punctuation:_ All non-alphanumeric characters
are stripped as part of the parsing process. Likewise, English pluralization is
accounted for and in the case of specific view templates, the word order is
irrelevant. Therefor, `CONTROLLERS` will act as `controller`; `API:controller`
will act as `api.controller`; `Views` will act as `view`; `Create.View`,
`viewcreate`, `view-CREATE` will all act as `view.create`.

### Installation
1. Add my github to the `repositories` section of your composer.json. You might
not have this section yet and need to create it.
```
"repositories": [
    {
        "type": "vcs",
        "url": "https://github.com/webbtj/crud"
    }
]
```
2. Composer install my package (`composer require webbtj/crud`);
3. Publish the config file (`php artisan vendor:publish`, select `crud-config`)
4. Edit the newly published `config/crud.php` file to include whatever models
you want.

### TODO
* [ ] Better documentation
* [ ] Testing
* [x] Allow users to inject middleware
* [x] API routing
* [x] Command to build _real_ controllers
* [x] Command to build _real_ views
