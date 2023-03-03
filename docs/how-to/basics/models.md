# Models

Models are one of the cool features of OffbeatWP. It makes "The Loop" obsolete and gives a much more intuitive way of getting data from a post or term like the title, content or url.

OffbeatWP has three important types of models:
1. [Post Models](post_models.md)
    A post model represents a post in a specific post type
2. [Term Models](term_models.md)
    A term model represents a term in a specific taxonomy
3. User Models
    A user model represents a user with one or more specific roles.

Here, we will describe some methods that are shared by all models.
In these examples we will use

## Retrieving an existing model from the database
The easiest way to retrieve a single model that already exists is throught he find method.
```php
ExampleModel::find(?int $id): ?ExampleModel;
```
This method will retrieve the model with the given ID, provided it matches the post type of the model it was called on.
If a post with the given ID does not exist or has a different post type, the method will return NULL.

The method expects a POSITIVE int as argument. If an int of 0 or lower is passed, it will immediately return NULL without doing a query.

## Retrieving metadata on a post
All models have helper methods that can be used to retrieve metadata as a specific type.
If the metadata does not exist, then the falsy version of the respective type is returned. (EG: An empty string if you call getMetaString)
If you want to check if the meta value exists on a post, use the hasMeta method.

Available methods:

```php
ExampleModel->getMetaString(string $metaKey): string;
ExampleModel->getMetaInt(string $metaKey): int;
ExampleModel->getMetaFloat(string $metaKey): float;
ExampleModel->getMetaBool(string $metaKey): bool;
ExampleModel->getMetaArray(string $metaKey, $single = true): array;
ExampleModel->getMetaDateTime(string $metaKey): ?WpDateTime;
ExampleModel->getMetaDateTimeImmuteable(string $metaKey): ?WpDateTimeImmuteable;
```
