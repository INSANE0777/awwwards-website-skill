# WordPress Adaptation Guide
## Converting an Awwwards-level HTML/React site into a custom WordPress theme

This is a platform adaptation task — NOT a redesign. Every visual, animation, interaction,
and layout decision from the original must survive the conversion intact.

---

## THEME FILE STRUCTURE

```
my-theme/
├── style.css           ← Theme header (required by WordPress)
├── functions.php       ← Enqueue scripts/styles, register menus, theme setup
├── index.php           ← Fallback template
├── front-page.php      ← Homepage template (used instead of index.php for front page)
├── header.php          ← <head> + opening <body> + site nav
├── footer.php          ← Closing scripts + </body></html>
├── page.php            ← Generic page template
├── single.php          ← Single post template
├── 404.php             ← Error page
├── assets/
│   ├── css/
│   │   └── main.css    ← All site styles (moved from <style> tags)
│   ├── js/
│   │   └── main.js     ← All site JS (moved from <script> tags)
│   └── fonts/          ← Self-hosted fonts if not using Google Fonts CDN
└── template-parts/
    ├── hero.php        ← Hero/panel section partial
    ├── panels.php      ← Panel grid partial
    └── footer-nav.php  ← Footer partial
```

---

## style.css — THEME HEADER (required)

```css
/*
Theme Name:   My Awwwards Theme
Theme URI:    https://example.com
Author:       Your Name
Author URI:   https://example.com
Description:  Custom cinematic portfolio theme
Version:      1.0.0
License:      GNU General Public License v2 or later
License URI:  http://www.gnu.org/licenses/gpl-2.0.html
Text Domain:  my-awwwards-theme
Tags:         portfolio, custom-menu, full-width-template
*/

/* All theme styles go here OR import from assets/css/main.css */
@import url('assets/css/main.css');
```

---

## functions.php — THE ENGINE

```php
<?php

// 1. Theme setup
function theme_setup() {
    // Enable post thumbnails
    add_theme_support('post-thumbnails');
    // Enable title tag management
    add_theme_support('title-tag');
    // Enable HTML5 markup
    add_theme_support('html5', ['script', 'style', 'gallery', 'caption']);

    // Register navigation menus
    register_nav_menus([
        'primary'   => __('Primary Navigation', 'my-awwwards-theme'),
        'footer'    => __('Footer Navigation', 'my-awwwards-theme'),
    ]);
}
add_action('after_setup_theme', 'theme_setup');


// 2. Enqueue styles and scripts (NEVER inline — always enqueue)
function theme_enqueue_assets() {

    // Main stylesheet
    wp_enqueue_style(
        'theme-main',
        get_template_directory_uri() . '/assets/css/main.css',
        [],
        '1.0.0'
    );

    // Google Fonts (editorial)
    wp_enqueue_style(
        'theme-fonts',
        'https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;1,400&family=Playfair+Display:wght@400;700&family=Pinyon+Script&family=Inter:wght@300;400;500&display=swap',
        [],
        null
    );

    // GSAP (from CDN, no version hash needed)
    wp_enqueue_script(
        'gsap',
        'https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js',
        [],
        null,
        true // load in footer
    );

    wp_enqueue_script(
        'gsap-scrolltrigger',
        'https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js',
        ['gsap'], // depends on gsap
        null,
        true
    );

    // Lenis smooth scroll
    wp_enqueue_script(
        'lenis',
        'https://cdn.jsdelivr.net/npm/@studio-freight/lenis@1.0.42/dist/lenis.min.js',
        [],
        null,
        true
    );

    // SplitType
    wp_enqueue_script(
        'splittype',
        'https://cdn.jsdelivr.net/npm/split-type@0.3.4/umd/index.min.js',
        [],
        null,
        true
    );

    // Main JS — depends on all above
    wp_enqueue_script(
        'theme-main',
        get_template_directory_uri() . '/assets/js/main.js',
        ['gsap', 'gsap-scrolltrigger', 'lenis', 'splittype'],
        '1.0.0',
        true // ALWAYS load in footer
    );

    // Pass PHP data to JS if needed (e.g. theme URL, ajax URL)
    wp_localize_script('theme-main', 'themeData', [
        'themeUrl' => get_template_directory_uri(),
        'ajaxUrl'  => admin_url('admin-ajax.php'),
        'siteUrl'  => get_site_url(),
    ]);
}
add_action('wp_enqueue_scripts', 'theme_enqueue_assets');


// 3. Preconnect for Google Fonts (performance)
function theme_resource_hints($urls, $relation_type) {
    if ('preconnect' === $relation_type) {
        $urls[] = ['href' => 'https://fonts.googleapis.com'];
        $urls[] = ['href' => 'https://fonts.gstatic.com', 'crossorigin' => true];
    }
    return $urls;
}
add_filter('wp_resource_hints', 'theme_resource_hints', 10, 2);


// 4. Custom post type for Works/Projects (if needed)
function register_works_cpt() {
    register_post_type('work', [
        'labels' => [
            'name'          => __('Works'),
            'singular_name' => __('Work'),
            'add_new_item'  => __('Add New Work'),
            'edit_item'     => __('Edit Work'),
        ],
        'public'       => true,
        'has_archive'  => true,
        'supports'     => ['title', 'editor', 'thumbnail', 'excerpt', 'custom-fields'],
        'menu_icon'    => 'dashicons-format-video',
        'rewrite'      => ['slug' => 'works'],
        'show_in_rest' => true, // enables Gutenberg + REST API
    ]);
}
add_action('init', 'register_works_cpt');


// 5. Custom taxonomy for Work categories
function register_work_taxonomy() {
    register_taxonomy('work_category', 'work', [
        'labels'       => ['name' => __('Categories'), 'singular_name' => __('Category')],
        'hierarchical' => true,
        'rewrite'      => ['slug' => 'work-category'],
        'show_in_rest' => true,
    ]);
}
add_action('init', 'register_work_taxonomy');


// 6. Remove WordPress bloat that conflicts with custom design
function theme_cleanup() {
    remove_action('wp_head', 'wp_generator');          // hide WP version
    remove_action('wp_head', 'wlwmanifest_link');
    remove_action('wp_head', 'rsd_link');
    remove_action('wp_head', 'wp_shortlink_wp_head');
}
add_action('init', 'theme_cleanup');
```

---

## header.php

```php
<!DOCTYPE html>
<html <?php language_attributes(); ?>>
<head>
  <meta charset="<?php bloginfo('charset'); ?>"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <?php wp_head(); // REQUIRED — outputs enqueued styles, meta tags, etc. ?>
</head>

<body <?php body_class(); ?>>
<?php wp_body_open(); // REQUIRED for plugins like cookie banners ?>

<!-- Grain overlay -->
<div class="grain" aria-hidden="true"></div>

<!-- Custom cursor -->
<div class="c-dot"  aria-hidden="true"></div>
<div class="c-ring" aria-hidden="true"></div>

<!-- Skip link (accessibility) -->
<a href="#main-content" class="skip-link">Skip to content</a>

<!-- Navigation -->
<header class="site-nav" role="banner">
  <div class="nav-left">
    <span class="material-symbols-outlined nav-icon">notifications</span>
  </div>

  <div class="nav-center">
    <a href="<?php echo esc_url(home_url('/')); ?>" class="site-logo">
      <?php bloginfo('name'); ?>
    </a>
  </div>

  <div class="nav-right">
    <button class="nav-menu-toggle" aria-label="Open menu" aria-expanded="false">
      <span class="material-symbols-outlined">menu</span>
    </button>

    <!-- WordPress nav menu — registered in functions.php -->
    <?php wp_nav_menu([
      'theme_location' => 'primary',
      'container'      => 'nav',
      'container_attr' => ['aria-label' => 'Main navigation', 'class' => 'nav-menu'],
      'menu_class'     => 'nav-menu__list',
      'fallback_cb'    => false,
    ]); ?>
  </div>
</header>

<main id="main-content" role="main">
```

---

## footer.php

```php
</main><!-- #main-content -->

<footer class="site-footer" role="contentinfo">
  <div class="footer-left">
    <?php wp_nav_menu([
      'theme_location' => 'footer',
      'container'      => false,
      'menu_class'     => 'footer-nav',
      'fallback_cb'    => false,
    ]); ?>
    <!-- Fallback if no menu set -->
    <a href="<?php echo esc_url(home_url('/works')); ?>" class="footer-link">See all works</a>
  </div>

  <div class="footer-right">
    <span class="text-micro opacity-50">
      &copy; <?php echo date('Y'); ?> <?php bloginfo('name'); ?>
    </span>
  </div>
</footer>

<?php wp_footer(); // REQUIRED — outputs enqueued scripts ?>
</body>
</html>
```

---

## front-page.php — HOMEPAGE

```php
<?php get_header(); ?>

<!-- Paper grain is in header.php -->

<!-- Loader -->
<div class="loader" aria-hidden="true">
  <span class="loader-count">0</span>
</div>

<!-- Hero panels section -->
<?php get_template_part('template-parts/panels'); ?>

<!-- Text block below panels -->
<div class="text-block" aria-live="polite">
  <p class="text-label">Explore Song &amp; Extra Material</p>
  <h2 class="text-title locked-title">Featured Work</h2>
  <p class="text-subtitle locked-subtitle">Select a work to explore</p>
</div>

<?php get_footer(); ?>
```

---

## template-parts/panels.php — DYNAMIC PANELS FROM CPT

```php
<?php
// Query 'work' custom post type for panel data
$works = new WP_Query([
    'post_type'      => 'work',
    'posts_per_page' => 10,
    'orderby'        => 'menu_order',
    'order'          => 'ASC',
    'post_status'    => 'publish',
]);
?>

<div class="panels-container" data-hover="0">

  <?php if ($works->have_posts()) :
    $i = 1;
    while ($works->have_posts()) : $works->the_post();
      $video_url = get_post_meta(get_the_ID(), 'panel_video_url', true);
      $is_center = get_post_meta(get_the_ID(), 'is_center_panel', true);
      $panel_class = $is_center ? 'hero-panel hero-panel--center' : 'hero-panel';
  ?>

    <div class="<?php echo esc_attr($panel_class); ?>"
         data-index="<?php echo $i; ?>"
         data-title="<?php echo esc_attr(get_the_title()); ?>"
         data-subtitle="<?php echo esc_attr(get_the_excerpt()); ?>"
         onmouseenter="onPanelEnter(this, <?php echo $i; ?>)"
         onmouseleave="onPanelLeave(this)">

      <?php if (has_post_thumbnail()) : ?>
        <img src="<?php the_post_thumbnail_url('full'); ?>"
             alt="<?php the_title_attribute(); ?>"
             loading="<?php echo $i <= 3 ? 'eager' : 'lazy'; ?>" />
      <?php endif; ?>

      <?php if ($video_url) : ?>
        <video loop muted playsinline>
          <source src="<?php echo esc_url($video_url); ?>" type="video/mp4" />
        </video>
      <?php endif; ?>

      <?php if ($is_center) : ?>
        <div class="center-glyph">
          <?php echo esc_html(get_post_meta(get_the_ID(), 'center_glyph', true) ?: 'M'); ?>
        </div>
      <?php endif; ?>

    </div>

  <?php $i++; endwhile; wp_reset_postdata(); endif; ?>

</div>
```

---

## page.php — GENERIC PAGE

```php
<?php get_header(); ?>

<article id="post-<?php the_ID(); ?>" <?php post_class('page-content'); ?>>
  <div class="page-inner">
    <header class="page-header">
      <h1 class="text-display"><?php the_title(); ?></h1>
    </header>
    <div class="page-body">
      <?php the_content(); ?>
    </div>
  </div>
</article>

<?php get_footer(); ?>
```

---

## ADDING CUSTOM FIELDS FOR PANELS

Use ACF (Advanced Custom Fields) plugin OR native custom meta:

**Option A: Native meta (no plugin needed)**
```php
// In functions.php — add meta box for Work posts
function add_work_meta_boxes() {
    add_meta_box(
        'work_panel_data',
        'Panel Data',
        'render_work_meta_box',
        'work',
        'normal',
        'high'
    );
}
add_action('add_meta_boxes', 'add_work_meta_boxes');

function render_work_meta_box($post) {
    $video   = get_post_meta($post->ID, 'panel_video_url', true);
    $glyph   = get_post_meta($post->ID, 'center_glyph',   true);
    $center  = get_post_meta($post->ID, 'is_center_panel', true);
    wp_nonce_field('work_panel_nonce', 'work_panel_nonce');
    ?>
    <p>
      <label>Video URL (MP4)</label><br/>
      <input type="url" name="panel_video_url" value="<?php echo esc_attr($video); ?>" style="width:100%"/>
    </p>
    <p>
      <label>Center Glyph Letter</label><br/>
      <input type="text" name="center_glyph" value="<?php echo esc_attr($glyph); ?>" maxlength="1"/>
    </p>
    <p>
      <label>
        <input type="checkbox" name="is_center_panel" value="1" <?php checked($center, '1'); ?>/>
        This is the center (anchor) panel
      </label>
    </p>
    <?php
}

function save_work_meta($post_id) {
    if (!isset($_POST['work_panel_nonce']) ||
        !wp_verify_nonce($_POST['work_panel_nonce'], 'work_panel_nonce')) return;
    if (defined('DOING_AUTOSAVE') && DOING_AUTOSAVE) return;
    if (!current_user_can('edit_post', $post_id)) return;

    update_post_meta($post_id, 'panel_video_url',  sanitize_url($_POST['panel_video_url'] ?? ''));
    update_post_meta($post_id, 'center_glyph',     sanitize_text_field($_POST['center_glyph'] ?? ''));
    update_post_meta($post_id, 'is_center_panel',  isset($_POST['is_center_panel']) ? '1' : '');
}
add_action('save_post_work', 'save_work_meta');
```

---

## CRITICAL RULES

**DO:**
- Use `wp_enqueue_script()` / `wp_enqueue_style()` — never hardcode `<script>` or `<link>` tags
- Always call `wp_head()` in header.php and `wp_footer()` in footer.php — plugins depend on these
- Use `get_template_directory_uri()` for asset paths — not hardcoded URLs
- Escape all output: `esc_html()`, `esc_attr()`, `esc_url()`, `the_title_attribute()`
- Use `wp_reset_postdata()` after every custom `WP_Query`
- Keep class names, data attributes, and JS hook selectors IDENTICAL to the original HTML

**DO NOT:**
- Inline `<script>` or `<style>` tags in templates (use enqueue system)
- Modify animation class names or data attributes (JS depends on them)
- Add WordPress nav wrapping divs that break the layout (control via `wp_nav_menu()` args)
- Use jQuery unless absolutely necessary (GSAP doesn't need it)
- Add `!important` overrides to fight WordPress default styles — remove the defaults instead

**REMOVE DEFAULT WP STYLES that conflict:**
```php
// In functions.php
function remove_wp_block_styles() {
    wp_dequeue_style('wp-block-library');       // Gutenberg blocks
    wp_dequeue_style('wp-block-library-theme'); // Gutenberg theme
    wp_dequeue_style('classic-theme-styles');   // Classic editor
    wp_dequeue_style('global-styles');          // Global styles
}
add_action('wp_enqueue_scripts', 'remove_wp_block_styles', 100);
```

---

## JAVASCRIPT ADAPTATION

Your `main.js` needs one WordPress-specific change — wrap everything in DOMContentLoaded
and reference `themeData` (passed via `wp_localize_script`) for dynamic paths:

```javascript
// assets/js/main.js
document.addEventListener('DOMContentLoaded', () => {

  // themeData is passed from PHP via wp_localize_script
  const themeUrl = window.themeData?.themeUrl || '';

  // All your existing JS — unchanged
  gsap.registerPlugin(ScrollTrigger);
  initLenis();
  initCursor();
  initLoader(() => {
    revealPanels();
    initMagneticButtons();
  });

  // etc — no changes to animation logic
});
```

---

## DEPLOYMENT CHECKLIST

- [ ] `style.css` has valid theme header (Name, Version, Text Domain)
- [ ] `wp_head()` called in header.php
- [ ] `wp_footer()` called in footer.php
- [ ] `wp_body_open()` called after `<body>` tag
- [ ] All assets enqueued via `wp_enqueue_script/style` — no inline tags
- [ ] Custom post type registered for Works
- [ ] Custom meta fields saving and retrieving correctly
- [ ] Navigation menus registered and assigned in Appearance → Menus
- [ ] Default block styles removed (no Gutenberg CSS leaking)
- [ ] All JS wrapped in `DOMContentLoaded`
- [ ] Class names and data attributes unchanged from original HTML
- [ ] Animations, hover effects, and transitions visually identical
- [ ] Mobile responsive behavior preserved
- [ ] `prefers-reduced-motion` CSS still present in main.css
