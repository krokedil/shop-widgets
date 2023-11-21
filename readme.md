## Krokedil Shop Widgets package.

This package contains classes to assist in adding HTML widgets to WooCommerce or WordPress, on any page.
The widgets are printed directly in the HTML markup where you want them to appear.

The package comes with 2 classes, one for the cart page, and another for product pages. There is also a Abstract class that can be used to create custom widgets if needed.

### Installation

Install the package with composer:

```bash
composer require krokedil/shop-widgets
```

### Usage
The implementation of the Cart and Product widget classes is exactly the same, and pretty straight forward.

```php
use Krokedil\ShopWidgets\CartWidget;

// First create a instance of the widget class, where you pass a slug for your plugin, and the settings from your plugin that uses the WC_Settings_Api class, ie. Gateways, Shipping methods etc.
$settings = get_option( 'my_plugin_settings', array() );
$cart_widget = new CartWidget( 'my_plugin', $settings );

// Then you need to set the output as a string, this will be the HTML that is printed.
$cart_widget->set_output( '<div class="my-plugin-widget">This is my widget</div>' );

// If you don't want to use the settings that come with the package, you can set a fixed hook and priority for the widget.
$cart_widget->set_hook( 'woocommerce_after_cart_totals' );
$cart_widget->set_priority( 10 );

// Finally you need to call the init() method to register the action so the widget will be printed.
$cart_widget->init();

// This can also be chained together as a fluid api.
$cart_widget
    ->set_output( '<div class="my-plugin-widget">This is my widget</div>' )
    ->set_hook( 'woocommerce_after_cart_totals' )
    ->set_priority( 10 )
    ->init();
```

If your widget requires either CSS or JS, you can add them to the widget class by using the add_style_handles() and add_script_handles() methods.
You just need to add the handles for the scripts and styles, however the registration you will need to do before hand in your own plugin.

```php
use Krokedil\ShopWidgets\CartWidget;

wp_register_style( 'my-widget-styles', 'path/to/styles.css' );
wp_register_script( 'my-widget-scripts', 'path/to/scripts.js' );

$cart_widget = new CartWidget( 'my_plugin', $settings );

// Add the styles and scripts
$cart_widget
    ->add_style_handles( array( 'my-widget-styles' ) )
    ->add_script_handles( array( 'my-widget-scripts' ) )
    ...
    ->init();
```

This will automatically enqueue these styles and scripts before the widget is printed. It will also make a check to ensure they are only enqueued on pages they are valid for.

The package also comes with some predefined settings that can be used to allow users to set the locations themselves.
You can access these settings by calling the get_setting_fields() method on the widget class, and pass the title you want the settings to have on your page.

```php
use Krokedil\ShopWidgets\CartWidget;

$cart_widget = new CartWidget( 'my_plugin', $settings );

// Get the settings fields
$settings_fields = $cart_widget->get_setting_fields( 'Cart widget settings' );

// Append them to the settings array you already have from your plugin.
$plugin_settings_fields = array_merge( $plugin_settings_fields, $settings_fields );
```

This will give you 5 settings for the cart and product widgets. The settings are:
- Enabled - Enable or disable the widget.
- Placement - Select box with predefined locations for the widgets, based on the WooCommerce hooks for those pages.
- Enable custom placement hook - Enables the user to enter a hook and priority manually for the widget.
- Custom placement hook - The hook where the widget should be printed if custom placement is enabled.
- Custom placement hook priority - The priority for the hook if custom placement is enabled.

If these settings are in place, they will be used automatically without any more code. However, if you set a hook and priority using the set_hook() and set_priority() methods, these will always be used instead of the settings.

### Documentation
All technical documentation for the package can be found [here](/docs/docs.md)
