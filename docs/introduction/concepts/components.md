After a component is made, you have to possibilty to call it in the twig template itself.
For example:

```twig
{{ component('componentName', {'posts' : posts, 'showResultButton' : true}) }}
```
