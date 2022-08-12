# Taxonomies

Taxonomies are one of the core elements of Wordpress to structure posts. 

Even though the default Wordpress way of registering taxonomies will still work with OffbeatWP, we would encourage you to use the OffbeatWP method to do it. It makes registering a taxonomy a lot more declarative. 

This is how registering a post-type in OffbeatWP looks like:

```php
offbeat('taxonomy')::make('download_category', ['download'], 'Categories', 'Category')
    ->model(Models\DownloadCategoryModel::class)
    ->hierarchical(true)
    ->notPubliclyQueryable()
    ->set();
```

Basically the first and last line are the most important. The first line:

```php
offbeat('taxonomy')::make('download_category', ['download'], 'Categorieën', 'Categorie')
```

has four arguments, the `taxonomy`, `post_types`, `Plural label` and a `Single label`

The last line (`->set()`) performs the actual "register_taxonomy".

This line in between are settings of the taxonomy.

## Register in service

Taxonomies should be registered in a Service or Module (what basically is a Service as well). We recommend it by doing it like this:

```php
class Downloads extends AbstractModule
{
    public function register()
    {
        add_action('init', [$this, 'registerContentTypes']);
    }

    public function registerContentTypes()
    {
        offbeat('taxonomy')::make('download_category', ['download'], 'Categories', 'Category')
            ->model(Models\DownloadCategoryModel::class)
            ->hierarchical(true)
            ->notPubliclyQueryable()
            ->set();

...
```

## Register the related model

When you register the taxonomy you can set the related model with it. By the OffbeatWP knows how to map the terms the right model.

```php
->model(Models\DownloadCategoryModel::class)
```


## Available methods for settings

```php
rewrite($rewrite)
```

```php
labels($labels)
```

```php
model($modelClass)
```

```php
hierarchical($hierarchical = false)
```

```php
notPubliclyQueryable()
```

```php
public($public = true)
```

```php
showUI($showUI = true)
```

```php
inMenu($menu)
```

```php
showAdminColumn($showAdminColumn = true)
```

```php
metaBox($metaboxCallback)
```

For more information about registering taxonomies check the Wordpress documentation about [register_taxonomy](https://codex.wordpress.org/Function_Reference/register_taxonomy)


