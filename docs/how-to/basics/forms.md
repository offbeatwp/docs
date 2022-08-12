# Forms

The `\OffbeatWP\Form\Form` is an abstract implementation that can be mapped to the right structure depending on the implementation you're using. The forms are currently used for Components and SiteSettings. 

## Form Structure

The current structure of a form is like this:
```
- Form
    - Tab
        - Section
            - Field
        - Repeater
            - Field
```

The use of tabs in a Component context is mandatory, but for SiteSettings, it isn't. A Section is not mandatory but really helps to structure your fields.

A more advanced example of a form:
```php
$form = new Form();

$columnsField = Select::make('title2', 'Title');
$columnsField->addOptions(['2' => '2', '3' => '3', '4' => '4'])

$form
    ->addTab('general', 'General')
        ->addSection('general', 'General')
            ->addField(Text::make('title', 'Title'))
            ->addField(Editor::make('content', 'Content'))
    ->addTab('Appearance', 'Appearance')
        ->addSection('appearance', 'Appearance')
            ->addField($columnsField)
        ->addRepeater('pros_list', 'Pro list')
            ->addField(Text::make('pro', 'Pro'));
```


## Fields

Below the fields that are currently been integrated:

### Text

```php
Text::make($id, $label)
```

### Textarea

```php
TextArea::make($id, $label)
```

### Editor

```php
Editor::make($id, $label)
```

### Select
The selected value will be returned as a string.

```php
Select::make($value, $label)
```

To define options you can first construct the field and then assign the options:

```php
$selectField = Select::make($id, $label);
$selectField->addOptions(['fieldValue' => 'Field Label']);

// or add single option:
$selectField->addOption('fieldValue', 'Field Label']);
```

Or chain it:
```php
$selectField = Select::make($id, $label)->addOptions(['fieldValue' => 'Field Label']);
```

### Image

```php
Image::make($id, $label);
```

### TrueFalse

```php
TrueFalse::make($id, $label);
```

### Post

Selecting a post

```php
$postField = Post::make($id, $label);

// if you only want from specific post types
$postField->fromPostTypes(['post']);
```

### Posts

Selecting multiple posts

```php
$postsField = Posts::make($id, $label);

// if you only want from specific post types
$postsField->fromPostTypes(['post']);
```

### Term

Selecting a term

```php
$termField = Term::make($id, $label);

// if you only want from specific taxonomies
$termField->fromTaxonomies(['post']);
```

### Terms

Selecting multiple terms

```php
$termsField = Terms::make($id, $label);

// if you only want from specific taxonomies
$termsField->fromTaxonomies(['post']);
```

### NavMenu

Selecting a nav menu

```php
NavMenu::make($id, $label);
```

### User

Selecting a user

```php
User::make($id, $label);
```

### Users

Selecting multiple users

```php
Users::make($id, $label);
```

### Breakpoints

Selecting one of the default bootstrap breakpoints

```php
Breakpoint::make($id, $label);
```

### TextAlign

Select box with text-align options

```php
TextAlign::make($id, $label);
```

### HorizontalAlign

Select box with horizontal align options

```php
HorizontalAlign::make($id, $label);
```

### VericalAlign

Select box with vertical-align options

```php
VericalAlign::make($id, $label);
```

## Creating your own fields

In some cases, it could be interesting to create your own fields by extending OffbeatWP Fields so you can re-use them. The right place to this is in an `app/Form/Fields` folder from the root of your theme.

If for example, you want to have a field to define a rating create a file named `Rating.php` in your `app/Form/Fields` directory, containing:

```php
<?php
namespace App\Form\Fields;

use \OffbeatWP\Form\Fields\Select;

class Rating extends Select {

    public function __construct()
    {        
        $this->addOptions([
            '' => __('No Rating', 'offbeatwp'),
            '1' => __('No Way', 'offbeatwp'),
            '2' => __('Mwah', 'offbeatwp'),
            '3' => __('It\'s ok', 'offbeatwp'),
            '4' => __('Pretty awesome actually', 'offbeatwp'),
            '5' => __('Awesomsaus', 'offbeatwp'),
        ]);

        parent::__construct();
    }
    
}
```

Within the form you do:

```php
$form->.......->addField(Rating::make($id, $label))
```

## Field Collections

Field collections are a combination of fields that can be reused, current collections available:

Adding a fields collection to a form must be done with the `->addFields` method.

```php
$form->.......->addFields(Heading::make())
```

### Heading

The heading fields collection automatically adds the following fields to your form:
- Title (text field)
- Heading Type (select box is h1-h6)
- Heading Style (select box is h1-h6)

```php
Heading::make();
```

### Link

The Link fields collection automatically adds the following fields to your form:
- Link label (text field)
- Link url (text field)
- Link target (selectbox is \_blank, \_self, \_top)

```php
Link::make();
```

## Defining your own Collections

In some cases, it could be interesting to create your own fields collection so you can reuse them. The right place to this is in an `app/Form/FieldsCollections` folder from the root of your theme.

```php
namespace App\Form\FieldsCollections;

use \OffbeatWP\Form\FieldsCollections\AbstractFieldsCollection;
use \OffbeatWP\Form\Fields\Text;

class SocialLinks extends AbstractFieldsCollection {
    public function __construct()
    {
        $this->addField(Text::make('facebook_link', __('Facebook link', 'offbeatwp')));
        $this->addField(Text::make('instagram_link', __('Instagram link', 'offbeatwp')));
        $this->addField(Text::make('twitter_link', __('Twitter link', 'offbeatwp')));
    }
}
```

To use this within the form:

```php
$form->.......->addFields(SocialLinks::make())
```

## Example of a form being used within a component
In this example, we want the user to be able to select a 'Person' which is a custom post type.
```php
class PersonCard extends AbstractComponent
{
    public static function settings(): array
    {
        return [
            'name' => __('Person Card', 'offbeatwp'),
            'slug' => 'person-card',
            'supports' => ['pagebuilder', 'editor']
        ];
    }

    public static function getForm(): Form {
        $form = new Form();

        $form->addField(Post::make('personId', __('Person', 'raow'))->fromPostTypes([PersonModel::POST_TYPE]));
        
        return $form;
    }

    public function render($settings)
    {
        return $this->view('person-card', [
            'settings' => $settings, 
            'person' => new PersonModel($settings->personId)
        ]);
    }
}
```