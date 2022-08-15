# Post types

Post and post types are one of the core elements of Wordpress, and one of the reasons Wordpress is so flexible. 

Even though the default Wordpress way of registering post types will still work with OffbeatWP, we would encourage you to use the OffbeatWP method to do it. It makes registering a post type a lot more declarative. 

This is how registering a post-type in OffbeatWP looks like:

```php
offbeat('post-type')::make('download', 'Downloads', 'Download')
    ->model(DownloadModel::class)
    ->supports(['title', 'editor'])
    ->icon('dashicons-format-aside')
    ->notPubliclyQueryable()
    ->inRest(false)
    ->public()
    ->set();
```

Basically the first and last line are the most important. The first line:

```php
offbeat('post-type')::make('download', 'Downloads', 'Download')
```

has three arguments, the `post_type`, `Plural label` a `Single label`

The last line (`->set()`) performs the actual "register_post_type".

This line in between are settings of the post_type.

## Register in service

Post types should be registered in a Service or Module (what basically is a Service as well). We recommend it by doing it like this:

```php
class Downloads extends AbstractModule
{
    public function register()
    {
        add_action('init', [$this, 'registerContentTypes']);
    }

    public function registerContentTypes()
    {
        offbeat('post-type')::make('download', 'Downloads', 'Download')
            ->model(DownloadModel::class)
            ->supports(['title', 'editor'])
            ->icon('dashicons-format-aside')
            ->notPubliclyQueryable()
            ->inRest(false)
            ->public()
            ->set();

...
```

## Register the related model

When you register the post_type you can set the related model with it. By the OffbeatWP knows how to map the posts the right model.

```php
->model(Models\DownloadModel::class)
```


## Available methods for settings

```php
rewrite($rewrite) ex: rewrite(['with_front' => false, 'slug' => 'client'])
```

```php
labels($labels)
```

```php
model($modelClass)
```

```php
isHierarchical($hierarchical = true)
```

```php
supports($support)
```

```php
notPubliclyQueryable()
```

```php
public($public = true)
```

```php
excludeFromSearch($exclude = true)
```

```php
showUI($showUI = true)
```

```php
icon($icon)
```

```php
inMenu($menu)
```

```php
inRest($showInRest = true)
```

```php
position($position = null)
```

```php
taxonomies(array $taxonomies)
```


For more information about register post type check the Wordpress documentation about [register_post_type](https://codex.wordpress.org/Function_Reference/register_post_type)

## Add admin columns for post type

### Static column

```php
addAdminTableColumn(string $name, string $label, string $modelFunc)
```

The `$modelFunc` should contain the function / method name from within the model. The return value of that method  will be displayed in the column.

### Sortable column based on meta

Easily add a sortabke column to the admin table based on a specific meta value.

```php
addAdminMetaColumn(string $metaName, string $label = '', string $orderBy = 'meta_value')
```
