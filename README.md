# WordPress-Custom-Codes

## In this Repo you get the Custom Codes for the WordPress Theme

> Function to Enqueue Custom CSS & Scripts
<!-- Enqueue Scripts -->
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

> Register a Custom Post Type
<!-- Register a Custom Post Type -->
    function create_post_type() {
        register_post_type('post_type_name',
            array(
                'labels' => array(
                    'name' => __('Post Type Name', 'Post Type General Name', 'text_domain'),
                    'singular_name' =>__('Post Type Singular Name', 'Post Type Singular Name', 'text_domain'),
                    'menu_name' => __('Post Type Menu Name', 'text_domain')
                ),
                'public' => true,
                'has_archive' => true,
                'rewrite' => array('slug' => 'post_type_name'),
                'supports' => array('title', 'editor', 'thumbnail', 'custom-fields', 'page-attributes'),
                'taxonomies' => array('category', 'post_tag')
            )
        );
    }
    add_action( 'init', 'create_post_type' );

> Create a Shortcode to display Custom Post Types
<!-- Shortcode to display Custom Post Types -->
    function showCPT(){
        $args = array(
            'post_type' => 'custom_post_type',
            'posts_per_page' => -1,
            'orderby' => 'title',
            'order' => 'ASC'
        );
        $postQuery = new WP_Query( $args );
        if( $postQuery->have_posts() ){
            $result = '';
            while( $postQuery->have_posts() ){
                $postQuery->the_post();
                $result .= '<div class="post-item">';
                $result .= '<a href="'.get_the_permalink().'">'.get_the_post_thumbnail().'</a>';
                $result .= '<h3><a href="'.get_the_permalink().'">'.get_the_title().'</a></h3>';
                $result .= '<p>'.get_the_excerpt().'</p>';
                $result .= '</div>';
            }
            wp_reset_postdata();
            return $result;
        }
    }
    add_shortcode('shortcode_name','showCPT');

> Shortcode to display all categories
<!-- Create a Shortcode to display all categories -->
    function showCategories(){
        $args = array(
            'orderby' => 'name',
            'order' => 'ASC',
            'hide_empty' => false,
            'class' => 'category-list'
        )
        $categories = get_categories( $args );
        $result = '<ul>';
        foreach( $categories as $category ){
            $result .= '<li><a href="'.get_category_link( $category->term_id ).'">'.$category->name.'</a></li>';
        }
        $result .= '</ul>';
        return $result;
    }
    add_shortcode('shortcode_name','showCategories');

> Add New User Role
<!-- Add New User Role -->
    add_role('new_user_role', 'New User Role', array(
        'read' => true,                       // true allows this capability
        'edit_posts' => true,                // true allows this capability
        'delete_posts' => false,             // false denies this capability
    ));

> Remove User Role
<!-- Remove User Role -->
    remove_role('new_user_role');

    $wp_roles = new WP_Roles();
    $wp_roles->remove_role('invetor');

> Redirect Specific User Role to Specific Page After Login
<!-- Redirect Specific User Role to Specific Page After Login -->
    function redirect_investor($redirect_to, $requested_redirect_to, $user) {
        global $user;
        if(isset($user->roles) && is_array($user->roles)) {

            /** Check if User is Admin */
            if(in_array('administrator', $user->roles)) {
                return $redirect_to;
            }
            
            /** Check if User is Investor */
            elseif(in_array('investor', $user->roles)) {
                return home_url('/investor-dashboard/');
            }
            
            /** Check if User is User */
            elseif(in_array('subscriber', $user->roles)) {
                return home_url();
            }

        }
    }
    add_filter('login_redirect', 'redirect_investor');

> Restrict Specific Page for Non-Logged In Users (Frontend)
<!-- Restrict Specific Page for Non-Logged In Users -->
    function restrict_page(){
        $global $post;
        if(is_page('page-name') && !is_user_logged_in() && !is_admin()){
            wp_redirect(home_url());
            exit;
        }
    }
    add_action('wp', 'restrict_page');

> Create Sidebar
<!-- Create Sidebar -->
    function sidebar_name(){
        register_sidebar(array(
            'name' => esc_html('Sidebar Name', 'text_domain'),
            'id' => 'sidebar-name',
            'description' => esc_html('Sidebar Description', 'text_domain'),
            'before_widget' => '<div id="%1$s" class="widget %2$s">',
            'after_widget' => '</div>',
            'before_title' => '<h3 class="widget-title">',
            'after_title' => '</h3>',
        ));
    }
    add_action('widgets_init', 'sidebar_name');
