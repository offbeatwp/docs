# What is OffbeatWP

OffbeatWP is a component-based (more on this later) theming framework for WordPress. But actually, it's a bit more than that. It uses the most modern WordPress technologies including:

- Composer
- Dependency Injection (using [PHP-DI](http://php-di.org/))
- Webpack for compiling assets
- MVC (model-view-controller)

It's our mission to create a more professional and standardized way of developing WordPress themes. Because WordPress doesn't guide you on how to structure your theme, themes get messy and very often slow.

Besides that, every developer has its habits which makes it hard to onboard new developers on the project of transferring the codebase to another developer.

## For who is OffbeatWP

OffbeatWP is made for agencies and other professionals who make custom websites for themselves and clients. Especially when you have a website with a large codebase and a wide variety of post-types and taxonomies you will experience OffbeatWP as a blessing.

If you are a creator of templates to sell themes (on ThemeForest for example) OffbeatWP is not (yet) the best choice because it doesn't support child themes yet. 
 
## Component-based

In a world where every page on a website is a 'landing page', it's important to have the ability to make every page attractive. WordPress by default offers templates, shortcodes, widgets, and blocks (Gutenberg) to build your website's pages. But very often you want to use the element in your template, as a shortcode, and as a widget. Here components fall into place. Components are abstract elements that can be called within a template but have also been mapped to a shortcode, widget, block (soon) of any other page builder element. This makes your theme much more maintainable.

[More on how to use components](concepts/components.md)

## Why do you want to work with OffbeatWP?

WordPress is an amazing open-source CMS system. But WordPress has some _bad_-sides, especially for custom-made websites. Below are some _bad_-sides of WordPress and how OffbeatWP is solving those (based on this [post](https://medium.com/track-changes/wordpress-without-shame-fedc1a2fef72)):

### #1: In WP “everything is a post”

Even though the "everything is a post" approach gives WordPress a lot of flexibility, sometimes it makes things a bit messy. OffbeatWP makes it possible to have a model per post-type helping you to structure your code and making it convenient to work with.

### #2: The Loop bites you in the ass at least a half dozen times per project.

Say goodbye to "The Loop". Since OffbeatWP uses Collections (from [Laravel](https://laravel.com/docs/5.7/collections)) and models you have easy access to the post\` data. As an example, you can get the permalink of the post. With as following:

```
post.permalink
```
### #3: It’s slow.

By default, WordPress isn't slow at all. Most of the time it is a combination of bad written themes and plugins, and not well configured hosting that makes WordPress slow. OffbeatWP can help you with professional writing your theme so only the code that needs to be executed is executed.

### #4: It’s really, really hard to get consistent caching logic.

In our humble opinion, you shouldn't use any *page caching* at all. It's our goal that every request to WordPress is handled and finished within 200ms. That's our budget. If a request exceeds this budget we have to optimize the request to make it happen.

### #5: There is no way of doing version control of any dependencies

OffbeatWP uses composer (for PHP packages) and npm (for asset building like scss, js, fonts, etc). These are version-controlled libraries. We encourage plugin developers to make their plugins also available as a _Service_. With this all the dependencies of your website will be defined in your theme, all versions controlled.

