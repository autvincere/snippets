# snippets
snippets for differents cms and frameworks

# WORDPRESS

//*************************************************************
WORDPRESS
//*************************************************************

//CUSTOM POSTYPE QUERY
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


//CUSTOM POSTYPE QUERY PARA CATEGORIA ESPECIFICA

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
    
//CIERRE CUSTOM POSTYPE QUERY PARA CATEGORIA ESPECIFICA
