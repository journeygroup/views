# Simple PHP Views

## What

A simple PHP class for creating lightweight renderable views for alacarte PHP frameworks. Template languages certainly have their place, but it turns out PHP is great at printing out strings too!

## Usage

### Installing

Composer is by far the easiest way to install Views.

```sh
composer require journey/views ~0.1
```

### Rendering Views

Creating and using views is easy-easy:

```php
// Here's one way:
$view = new Journey\View('template-file', $variables);
echo $view->render();

// Here's another:
$view = Journey\View::make('template-file', $variables);
echo $view->render();

// The quickest
echo Journey\View::make('template-file', $variables);
```

Key value pairs in the $variables array will be extracted for use in the template file. You can also choose to prefix your variables with the `variable_prefix` option. Of course views can easily be nested as well. Here's an example of typical usage:

```php
// file: app.php	
$view = Journey\View('master');

// ... some application logic
$variables = array(
	'title' => 'Hello World',
	'paragraph' => 'Lorum ipsum...'
);
$view->content = Journey\View::make('page', $variables);
```


```php
// file: templates/master.php
<!DOCTYPE html>
<html>
	<head>
		<title>Example Website</title>
	</head>
	<body>
		<?= $content ?>
	</body>
</html>
```

```php
// file: template/page.php
<h1><?= $title ?></h1>
<p><?= $paragraph ?></p>
```

### Configuration

Generally you'll want to configure the default values for your entire installation sometime around boot:

```php
Journey\View::defaults([
    'templates' => getcwd() . "/templates",
    'extension' => '.php',
    'variable_prefix' => null,
    'string_template' => false
]);
```

However you can also set per-instance configuration values:

```php
// Lets render the file /tmp/temporary-file.php
$view = new Journey\View('temporary-file');
$view->config(['templates' => '/tmp']);
echo $view;
```