# Classes and Objects

## Introduction to Classes and Objects

In WordPress, just like in any other object-oriented PHP application, classes and objects form the foundation of OOP. A class is a blueprint for creating objects, providing initial values for state (member variables or properties), and implementations of behavior (member functions or methods). An object is an instance of a class.

## Defining a Class

Defining a class in PHP involves using the `class` keyword followed by the class name and a pair of curly braces containing the properties and methods.

### Example

```php
<?php
class MyClass {
    public $property;

    public function __construct($property) {
        $this->property = $property;
    }

    public function myMethod() {
        echo $this->property;
    }
}
?>
```

## Creating Objects

Once a class is defined, you can create objects (instances of the class) using the `new` keyword.

### Example

```php
<?php
$object = new MyClass('Hello, World!');
$object->myMethod(); // Output: Hello, World!
?>
```

## Using Constructors

A constructor is a special method that is automatically called when an object is instantiated. Constructors are useful for any initialization that the object may need before it is used.

### Example

```php
<?php
class MyClass {
    public $property;

    public function __construct($property) {
        $this->property = $property;
    }

    public function myMethod() {
        echo $this->property;
    }
}

$object = new MyClass('Hello, World!');
$object->myMethod(); // Output: Hello, World!
?>
```

## Class Methods

Methods are functions that are defined inside a class. They define the behaviors of the objects created from the class.

### Example

```php
<?php
class MyClass {
    public $property;

    public function __construct($property) {
        $this->property = $property;
    }

    public function myMethod() {
        echo $this->property;
    }
}
?>
```

## Properties

Properties are variables that belong to a class. They define the attributes of the objects created from the class.

### Example

```php
<?php
class MyClass {
    public $property;

    public function __construct($property) {
        $this->property = $property;
    }
}
?>
```

## Static Properties and Methods

Static properties and methods belong to the class itself rather than to any specific object. They can be accessed without creating an instance of the class.

### Example

```php
<?php
class MyClass {
    public static $staticProperty = 'Static Property';

    public static function staticMethod() {
        echo self::$staticProperty;
    }
}

MyClass::staticMethod(); // Output: Static Property
?>
```

## Constants

Constants are class properties that cannot be changed. They are defined using the `const` keyword.

### Example

```php
<?php
class MyClass {
    const CONSTANT = 'Constant Value';

    public function displayConstant() {
        echo self::CONSTANT;
    }
}

$object = new MyClass();
$object->displayConstant(); // Output: Constant Value
?>
```

## Examples

### Custom Post Type Class

Creating a class to handle custom post types in WordPress can encapsulate all related functionalities in one place.

### Example

```php
<?php
class CustomPostType {
    public $postType;

    public function __construct($postType) {
        $this->postType = $postType;
        add_action('init', [$this, 'registerPostType']);
    }

    public function registerPostType() {
        register_post_type($this->postType, [
            'label' => ucfirst($this->postType),
            'public' => true,
            'supports' => ['title', 'editor', 'thumbnail'],
        ]);
    }
}

$bookPostType = new CustomPostType('book');
?>
```

### Shortcode Class

Creating a class to handle shortcodes helps to organize shortcode logic and makes it easier to manage.

### Example

```php
<?php
class Shortcode {
    public $tag;

    public function __construct($tag) {
        $this->tag = $tag;
        add_shortcode($this->tag, [$this, 'render']);
    }

    public function render($atts, $content = null) {
        return '<div class="shortcode">' . $content . '</div>';
    }
}

$exampleShortcode = new Shortcode('example');
?>
```

### Widget Class

Creating a custom widget by extending the `WP_Widget` class allows for encapsulating widget-related functionalities.

### Example

```php
<?php
class CustomWidget extends WP_Widget {
    public function __construct() {
        parent::__construct(
            'custom_widget',
            __('Custom Widget', 'text_domain'),
            ['description' => __('A Custom Widget', 'text_domain')]
        );
    }

    public function widget($args, $instance) {
        echo $args['before_widget'];
        echo $args['before_title'] . apply_filters('widget_title', 'Custom Widget') . $args['after_title'];
        echo 'Hello, World!';
        echo $args['after_widget'];
    }

    public function form($instance) {
        // Widget form in the admin area
    }

    public function update($new_instance, $old_instance) {
        // Update widget options
    }
}

function register_custom_widget() {
    register_widget('CustomWidget');
}
add_action('widgets_init', 'register_custom_widget');
?>
```

### Admin Page Class

Creating a class to handle admin pages in WordPress helps to organize the logic and keep the code clean.

### Example

```php
<?php
class AdminPage {
    public $pageTitle;
    public $menuTitle;
    public $capability;
    public $menuSlug;
    public $callback;

    public function __construct($pageTitle, $menuTitle, $capability, $menuSlug, $callback) {
        $this->pageTitle = $pageTitle;
        $this->menuTitle = $menuTitle;
        $this->capability = $capability;
        $this->menuSlug = $menuSlug;
        $this->callback = $callback;

        add_action('admin_menu', [$this, 'addAdminPage']);
    }

    public function addAdminPage() {
        add_menu_page(
            $this->pageTitle,
            $this->menuTitle,
            $this->capability,
            $this->menuSlug,
            $this->callback
        );
    }

    public function displayPage() {
        echo '<h1>' . $this->pageTitle . '</h1>';
        echo '<p>Welcome to the admin page!</p>';
    }
}

$adminPage = new AdminPage('My Admin Page', 'Admin Page', 'manage_options', 'my-admin-page', [$adminPage, 'displayPage']);
?>
```