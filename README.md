# WordPress-Custom-Codes

## In this Repo you get the Custom Codes for the WordPress Theme.


> Function to Enqueue Custom CSS & Scripts
    function add_custom_css_script() {
        /** Enqueue Custom CSS */
        wp_enqueue_style( 'custom-css', get_template_directory_uri() . '/css/custom.css' );

        /** Enqueue Custom Fonts */
        wp_enqueue_style('font-awesome', 'https://use.fontawesome.com/releases/v5.8.2/css/all.css');
        wp_enqueue_style('montserrat-fonts', 'https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,300;0,400;0,500;0,600;0,700;1,400;1,500&display=swap');

        /** Enqueue Custom Scripts */
        wp_enqueue_script('script', get_template_directory_uri() . '/script.js');
    }
    add_action( 'wp_enqueue_scripts', 'add_custom_css_script' );
