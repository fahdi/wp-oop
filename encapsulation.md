# Encapsulation

## Introduction to Encapsulation

Encapsulation is one of the fundamental principles of object-oriented programming. It refers to the bundling of data (variables) and methods (functions) that operate on the data into a single unit, known as a class. Encapsulation also restricts direct access to some of an object's components, which is a means of preventing unintended interference and misuse of the data. This is typically achieved using access modifiers.

## Public, Private, and Protected Access Modifiers

Access modifiers define the visibility of properties and methods in a class. PHP supports three access modifiers:

- **Public**: Members declared as public can be accessed from anywhere.
- **Protected**: Members declared as protected can be accessed within the class itself and by inheriting and parent classes.
- **Private**: Members declared as private can only be accessed within the class itself.

### Example

```php
<?php
class MyClass {
    public $publicProperty = 'Public';
    protected $protectedProperty = 'Protected';
    private $privateProperty = 'Private';

    public function displayProperties() {
        echo $this->publicProperty; // Accessible
        echo $this->protectedProperty; // Accessible
        echo $this->privateProperty; // Accessible
    }
}

$object = new MyClass();
echo $object->publicProperty; // Accessible
// echo $object->protectedProperty; // Error: Cannot access protected property
// echo $object->privateProperty; // Error: Cannot access private property

$object->displayProperties(); // Accessible
?>
```

## Getters and Setters

Getters and setters are methods used to access and update the values of private and protected properties. They provide a controlled way of accessing and modifying the properties.

### Example

```php
<?php
class MyClass {
    private $property;

    public function setProperty($value) {
        $this->property = $value;
    }

    public function getProperty() {
        return $this->property;
    }
}

$object = new MyClass();
$object->setProperty('Hello, World!');
echo $object->getProperty(); // Output: Hello, World!
?>
```

## Examples

### Encapsulating Custom Post Type Logic

Encapsulation can be used to bundle all the functionality related to a custom post type into a single class, making the code more modular and easier to maintain.

### Example

```php
<?php
class CustomPostType {
    private $postType;

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

    public function setPostType($postType) {
        $this->postType = $postType;
    }

    public function getPostType() {
        return $this->postType;
    }
}

$bookPostType = new CustomPostType('book');
?>
```

### Encapsulating Widget Logic

Similarly, encapsulation can be used to bundle all the functionality related to a custom widget into a single class.

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