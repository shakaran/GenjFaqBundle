# ShakaranFaqBundle

The ShakaranFaqBundle is a fork of GenjFaqBundle which include support for Symfony 4.3+.

It allows you to display a FAQ on your website, with the questions being grouped in categories. Features:

* Questions are grouped into Categories
* Categories can be deactivated
* Questions can be drafted and scheduled for publishing
* Can show all information at once, or collapse questions/categories for big FAQs.
 It's basically up to you - how you are handling this in the template
* Collapsed mode generates SEO friendly URLs
* Contains very simple mysql search - if you need it more advanced use elasticsearch

## Migration from GenjFaqBundle in your code

Execute the following commands if you are in a GNU/Linux terminal for easy and quick replacement
in your code:

```
grep -rl GenjFaqBundle src | xargs sed -i 's$GenjFaqBundle$ShakaranFaqBundle$g'
grep -rl GenjFaqBundle config | xargs sed -i 's$GenjFaqBundle$ShakaranFaqBundle$g'
grep -rl Genj src | xargs sed -i 's$Genj$Shakaran$g'
grep -rl Genj config | xargs sed -i 's$Genj$Shakaran$g'
```

## Requirements

see composer.json


## Installation

Add this to your composer.json:

```
    ...
    "require": {
        ...
        "gedmo/doctrine-extensions": "~2.3,<3.0",
        "shakaran/faq-bundle": "dev-master"
        ...
```

Then run `composer update`. After that is done, enable the bundle in your AppKernel.php:

```
# app/AppKernel.php
class AppKernel extends Kernel
{
    public function registerBundles() {
        $bundles = array(
            ...
            new Shakaran\FaqBundle\ShakaranFaqBundle()
            ...
```

Add the routing rules to your routing.yml:

```
# app/config/routing.yml
shakaran_faq:
    resource: "@ShakaranFaqBundle/Resources/config/routing.yml"
```

Finally, update your database schema:

```
php bin/console doctrine:schema:update --dump-sql
```

use the ```--force``` option to actually execute the DB update.

And you're done.
You should now be able to reach the bundle under the http://yourproject.com/faq URL
if you did add at least one category in your DB.


*Optional: loading fixtures*

If you use the doctrine-fixtures bundle, you can load fixtures like this:

```
php bin/console doctrine:fixtures:load --fixtures=vendor/shakaran/faq-bundle/src/Shakaran/FaqBundle/DataFixtures/
```


## Configuration

You can optionally include the configuration below into your config.yml:

```
shakaran_faq:
    select_first_category_by_default: false
    select_first_question_by_default: false
```

Both configuration will open the first category and/or question by default if the user has not
chosen a category and/or question yet. The default for both values is 'false', so set them
to 'true' if you want this behaviour.

Note that it is also required to have the Sluggable and Timestampable behaviours configured for
gedmo/doctrine-extensions (see https://github.com/Atlantic18/DoctrineExtensions).


## Advanced

As soon you want more than the default category listing with all questions + answers you
shouldn't import the bundle route, but copy only the part you are actualy using.

e.g. if you want single pages for each question use the ```shakaran_faq_question_show``` route.

For further examples see https://github.com/genj/faq-demo


## FAQ

* How do I add this to SonataAdmin?

You can use the GenjFaqAdminBundle:
https://github.com/genj/GenjFaqAdminBundle
or just create your own admin class.
