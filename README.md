Netzmacht Contao Form Bundle
============================

This bundle enables the power of Symfony forms in your Contao project.

The purpose if not (yet) to provide a fully replacement of Contao Core form handling but to enable you to use form 
configurations (form generator, data containers) in your application. 

Features
--------

 - Enables the symfony form component in your application
 - Provides a FormGeneratorType to use form based on the backend
 - Provides a HtmlType to add plain html to a form
 - Provides a backend form theme
 - Adds option for the widget allowing to define `class`, `fe_class`, `be_class` css attributes
 
### Form generator

If you want to use the form generator to configure your symfony forms - you can do it. This extension
ships a FromGeneratorType.

**Usage**

```php
// Symfony form generator, provided as service form.factory
$form = $formFactory->create(Netzmacht\ContaoFormBundle\Form\FormGeneratorType::class, null, ['formId' => 5]);

// That's all. Now you can use a symfon form having your form generator form fields.

```

### DCA forms

If you want to create forms based on the data container arrays - you can do it. The extension ships a DcaFormType.

**Usage**

```php
// Symfony form generator, provided as service form.factory
assert($dc instanceof Contao\DataContainer);
$form = $formFactory->create(
    Netzmacht\ContaoFormBundle\Form\DcaFormType::class, 
    null, 
    ['dataContainer' => 'tl_content', 'fields' => ['text'], 'driver' => $dc]
);
```

**Warning** The support for dca forms is experimental and might have some unforseen issues. It's highly recommended to 
pass the data container driver as callbacks might need them. Following types are supported so far:

 - `checkbox`
 - `password`
 - `radio`
 - `select`
 - `text`
 - `textarea`

### Backend form theme

This bundle also provides a form theme for the Contao backend. You can enable it in your twig template where the form is
used:

```twig
{% form_theme form '@NetzmachtContaoForm/form/contao_backend.html.twig' %}
{{ form(form) }}
```

### CSS attributes

Inside the form theme you can use additional classes to add specific styles, this is done by the `contaoWidget` option 
inside a FormType, there is a generic key `class` and context specific keys for frontend 
`fe_class` and backend `be_class`:

```php
$builder
    ->add(
        'field',
        FieldType::class,
        [
            'contaoWidget' => ['class' => 'generic-class', 'be_class' => 'clr w50', 'fe_class' => 'frontend-class'],
        ]
    )
```

### Input filtering

Be aware that the **Contao input sanitizing is bypassed by default**. If you need the data filtered, especially when 
using it in legacy context (f.e. Contao templates) you can filter the data by using the provided input filter
`Netzmacht\ContaoFormBundle\Filter\ContaoInputFilter` which is provided as service `netzmacht.contao_form.input_filter`.

### Upload handler

By default the uploaded file is available as `UploadedFile` instance in the form data. If you want to apply the 
configured setting form the form field, you might use the `Netzmacht\ContaoFormBundle\Form\FormGenerator\UploadHandler` 
class provided as service `netzmacht.contao_form.form_generator.upload_handler`.

Roadmap
-------

 - Support popular 3rd party form fields
 
Known limitations
-----------------

 - Unsupported form fields are just ignored (form generator) 
 - Submit buttons with images are not supported.
