Deferred Twig Extension
=======================

An extension for Twig that allows to defer block rendering.

## Initialization

```php
$twig = new Twig_Environment($loader);
$twig->addExtension(new Phive\Twig\Extensions\Deferred());
```

## Usage example

Let's assume that we have the following set of templates:

```jinja
{# layout.html.twig #}
<!DOCTYPE html>
<html>
    <head>
        ...
        {% deferred javascripts %}
            {% for item in storage %}
                <script src="{{ item }}"></script>
            {% endfor %}
        {% enddeferred %}
    </head>
    <body>
        {% block content '' %}
        {{ storage.append('/js/layout.js') }}
    </body>
</html>
```

```jinja
{# page.html.twig #}
{% extends "layout.html.twig" %}

{% block content %}
    {% if foo is not defined %}
        {{ include("subpage1.html.twig") }}
    {% else %}
        {{ include("subpage2.html.twig") }}
    {% endif %}

    {{ storage.append('/js/page.js') }}
{% endblock %}
```

```jinja
{# subpage1.html.twig #}
{{ storage.append('/js/subpage1.js') }}
```

```jinja
{# subpage2.html.twig #}
{{ storage.append('/js/subpage2.js') }}
```

> The `storage` is a [global twig variable](http://twig.sensiolabs.org/doc/advanced.html#globals)
> which can be created like this:
>
>     $twig = new Twig_Environment($loader);
>     $twig->addGlobal('storage', new ArrayObject());
>
> It's not provided by this extension and it's there just to show the order in which data are added.
>
> It's up to you how to share data between templates.

Then the output will be:

```html
<!DOCTYPE html>
<html>
    <head>
        ...
        <script src="/js/page.js"></script>
        <script src="/js/subpage1.js"></script>
        <script src="/js/layout.js"></script>
    </head>
    <body>
    </body>
</html>
```

## License

Deferred Twig Extension is released under the MIT License. See the bundled LICENSE file for details.
