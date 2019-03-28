# snippets
snippets for differents cms and frameworks

## WORDPRESS

### Cambio de cotejamiento en base de datos
Query para insertar en base de datos
```
// query **************************************************
//*************************************************************** */

ALTER DATABASE `fujicorp_fujicorp` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_520_ci;

// CIERRE QUERY **************************************************
//*************************************************************** */
```

### Consultar el charset y collation de una base de datos
Query para insertar en base de datos
```
// query **************************************************
//*************************************************************** */

SELECT
 schema_name AS 'database', 
 default_character_set_name AS 'charset',
 default_collation_name AS 'collation'
FROM
 information_schema.SCHEMATA
WHERE
 schema_name = "XXXXX";

// CIERRE QUERY **************************************************
//*************************************************************** */
```
### Registro de Custom POST-TYPE
```
// ITEM CARGA JSON **************************************************
//*************************************************************** */


function crear_json() {
  register_post_type( 'json',
    array(
      'labels' => array(
        'name' => __( 'Json' ),
        'singular_name' => __( 'Json' )
      ),
      'supports' => array(
          'thumbnail','custom-fields','title','excerpt'
      ),
      'public' => true,
      'has_archive' => false,
    )
  );
}

add_action( 'init', 'crear_json' );

// CIERRE ITEM CARGA JSON **************************************************
//*************************************************************** */
```

### Registro de Taxonomia para custom POST-TYPE
```
function json_taxonomy() {

  $labels = array(
  'name'                       => _x( 'categorias', 'Taxonomy General Name', 'theme' ),
  'singular_name'              => _x('categorias', 'Taxonomy Singular Name', 'theme' ),
  'menu_name'                  => __('categorias', 'theme' ),
  'all_items'                  => __( 'ver todos', 'theme' ),
  'parent_item'                => __( 'superior', 'theme' ),
  'parent_item_colon'          => __( 'slider superior', 'theme' ),
  'new_item_name'              => __( 'Nuevo nombre del pan', 'theme' ),
  'add_new_item'               => __( 'Agregar categoria', 'theme' ),
  'edit_item'                  => __( 'editar slider', 'theme' ),
  'update_item'                => __( 'Actualizar slider', 'theme' ),
  'view_item'                  => __( 'Ver slider', 'theme' ),
  'separate_items_with_commas' => __( 'separar sliders con coma', 'theme' ),
  'add_or_remove_items'        => __( 'agregar o remover sliders', 'theme' ),
  'choose_from_most_used'      => __( 'Mostrar los mas usados', 'theme' ),
  'popular_items'              => __( 'sliders populares', 'theme' ),
  'search_items'               => __( 'Buscar sliders', 'theme' ),
  'not_found'                  => __( 'No encontrado', 'theme' ),
  'no_terms'                   => __( 'No hay sliders', 'theme' ),
  'items_list'                 => __( 'lista de sliders', 'theme' ),
  'items_list_navigation'      => __( 'Items list navigation', 'theme' ),
  );
  $args = array(
  'labels'                     => $labels,
  'hierarchical'               => true,
  'public'                     => true,
  'show_ui'                    => true,
  'show_admin_column'          => true,
  'show_in_nav_menus'          => true,
  'show_tagcloud'              => false,
  );
  register_taxonomy( 'categorias', array( 'json' ), $args );
  
  }
  add_action( 'init', 'json_taxonomy', 0 );
  
```

### QUERY Custom POST-TYPE
```
// EJ: SLIDER

<section class="slider">
    <!-- slider custom postype -->

    <?php
    $args = array( 
        'post_type' => 'slider',
        'orderby' => 'menu_order',
        "order" => 'ASC',
        'posts_per_page'=> -1
    );

    $slider = new WP_Query( $args );

    if ($slider->have_posts() ) : ?>

    <div class="flexslider-container">
        <div id="catalogo-home" class="flexslider">
            <ul class="slides">
                <?php while ($slider->have_posts() ) : $slider->the_post(); 

                // campos slider post-type
                $imagen_slider = get_field('imagen_slider');
                ?>
                
                <li> 
                    <?php if (get_field('imagen_slider') != ''): ?>
                    <img src="<?php the_field('imagen_slider'); ?>" /> 
                    <?php endif; ?>
                </li>
                   
                    <?php endwhile; wp_reset_postdata(); ?>
            </ul>
        </div>
    </div>
    <?php endif; ?>

    <!-- Cierre slider custom postype -->
</section>
// CIERRE EJ: SLIDER
```

### QUERY Custom POST-TYPE para categoria especifica

```
<?php
	//OBTENER json de custom postype

		$args = array(
			'post_type' => 'json',
			'posts_per_page'  => 1,
			
			'tax_query' => array(
				array(
					'taxonomy' => 'categorias',
					'field' => 'slug',
					'terms' => 'json_produccion'
					)
			 ) 
		 );	
	
	$category_posts = new WP_Query($args);

	if($category_posts->have_posts()) : 
     	while($category_posts->have_posts()) : 
			$category_posts->the_post();
			$json = get_field('json'); 
     	?>
			<script>  
			 const
				url = '<?php echo $json; ?>';
				console.log(url);
			</script> 
     	<?php
               endwhile;
               else: 
          ?>
               Vaya, no hay entradas.
          <?php
           	endif;
          ?>
	  <!-- cierre obtener json de custom postype -->
```

