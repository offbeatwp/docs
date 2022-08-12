# Relationships (beta)

With a more advanced website, you often want to create relationships between posts. With OffbeatWP this is really simple.

First, we have to install a service, this to your `config/services.php` file:

```php
OffbeatWP\Content\Post\Relations\Service::class,
```

To make these queries run quickly we have to install an extra table in our database. This is done through WP-CLI, run from your terminal:

```bash
wp post-relations:install
```

## Defining relationships

In your model, you can define the relationship like this

```php
<?php
namespace App\Models;

use OffbeatWP\Content\Post\PostModel;

class EventPostModel extends PostModel {
    public const POST_TYPE = 'event';
        
    public function locations(): HasMany
    {
        return $this->hasMany('{key}');
    }
}
```

The key is something you make up yourself to identify the relationship. We recommend something declarative like the post type "event_location".

Besides `->hasMany` you can use `->hasOne()`, `->belongsTo()` or `->belongsToMany()`

## Attaching and Detaching relationships (for hasOne and hasHany)

Now you can attach another post by doing:

```php
$event->locations()->attach($id);
```

If you also use an array of ids to attach multiple posts at once. By default, the relations are appended to existing relationships. If you what to replace the existing relationships, you just have to add a second parameter with the value `false` like:

```php
$event->locations()->attach([1,2,3], false);
```

You can detach an relationship by doing:

```php
$event->locations()->detach($id);
```

or to detach all relationships.

```php
$event->locations()->detachAll();
```

## Associating and Dissociating relationships (for belongsTo and belongsToMany)

Now you can associate another post by doing:

```php
$location->events()->associate($id);
```

If you also use an array of ids to associate multiple posts at once. By default, the relations are appended to existing relationships. If you what to replace the existing relationships, you just have to add a second parameter with the value `false` like:

```php
$location->events()->associate([1,2,3], false);
```

You can dissociate an relationship by doing:

```php
$event->locations()->dissociate($id);
```

or to dissociate all relationships.

```php
$event->locations()->dissociateAll();
```


## Using relationships
```php
$event->locations()->get();
```
