# WordPress Custom Query - Watch & Learn

- [Watch & Learn series](https://www.youtube.com/watch?v=Y5rvMw3EH24&list=PLUBR53Dw-Ef8RvXF4iEU6UnhOlrcfIYn6&index=1)

## Basics

- our basic query takes some arguments to setup

```php
$args = array(
    'post_type' => 'post',
    'posts_per_page' => 5,
);

$query = new WP_Query($args);

while($query->have_posts()) : $query->the_post();
```

- great reference for all the many query arguments on [Bill Ericksons site](https://www.billerickson.net/code/wp_query-arguments/)
- we can run 2 separate queries and make sure the first query posts don't show in the second query by getting the ID of the post in our first query then making sure that ID is not shown in the next

```php
// getting our first query ID
$dontshowthisguy = $post->ID;

// then in our second query add that variable to the arguments
'post__not_in' => array($dontshowthisguy),
```

## Sorting

- we can sort based on a meta key when we setup custom fields

```php
$args = array(
    'post_type' => 'post',
    'posts_per_page' => 20,
    'meta_key' => 'order',
    'orderby' => 'meta_value',
    'order' => 'ASC'
);
```

## Comparing

- building on our sorting we can also use the meta_compare option, here we are pulling in posts with the name field that doesn't have 'John' as the value

```php
'meta_key' => 'name',
'meta_compare' => '!=',
'meta_value' => 'John',
```

## Taxonomy

```php
// we can filter by a category id:
'cat' => 17

// we can also filter by category name:
'category_name' => 'apples', 'oranges',

// filtering posts that have both our specific IDs:
'category__and' => array(17,20),

// filtering posts that at least one of these IDs:
'category__in' => array(17,20, 15),

// Exclude a category
// don't need to use an array but probably easier to change if needed
'category__not_in' => array(17, 20),

// TAGS

// Filter by a tag name:
'tag' => 'css',
// Filter by a tag ID:
'tag_id' => 10,
// Can also use and & in
'tag__and' => array(17,20),
'tag__in' => array(17,20),
'tag__not_in' => array(17,20),

// filter by slug name:
'tag_slug__and' => array('css','craft'),
'tag_slug__in' => array('css','craft'),
```

## meta_query

- this is a way to query multiple meta fields on our posts
- here we filter by xl size & price below 100

```php
$args = array(
    'post_type' => 'post',
    'posts_per_page' => -1,
    'meta_query' => array(
      'relation' => 'AND', // define the relation between the query OR, AND is default
      array(
        'key' => 'size',
        'value' => 'xl',
        'type' => 'CHAR',
        'compare' => '=',
      ),
      array(
        'key' => 'price',
        'value' => 100,
        'type' => 'NUMERIC',
        'compare' => '<'
      )
    )
);
```

- or we can filter between values

```php
$args = array(
    'post_type' => 'post',
    'posts_per_page' => -1,
    'meta_query' => array(
      array(
        'key' => 'price',
        'value' => array(50, 100),
        'type' => 'NUMERIC',
        'compare' => 'BETWEEN'
      )
    )
);
```

- we can also compare multple values in additional arrays - large or green & below 100

```php
$args = array(
    'post_type' => 'post',
    'posts_per_page' => -1,
    'meta_query' => array(
        array(
            'relation' => 'OR',
            array(
                'key' => 'size',
                'value' => 'l',
                'type' => 'CHAR',
                'compare' => '='
            ),
            array(
                'key' => 'color',
                'value' => 'green',
                'type' => 'CHAR',
                'compare' => '='
            )

        ),

        array(
            'key' => 'price',
            'value' => 100,
            'type' => 'NUMERIC',
            'compare' => '<'
        )
    )
);
```

## tax_query

- this allows us to make more complex queries using mulitple taxonomies, including custom taxonomies

```php
$args = array(
    'post_type' => 'post',
    'posts_per_page' => -1,
    'tax_query' => array(
        'relation' => 'AND',
        array(
            'taxonomy' => 'genre', // a custom taxonomy
            'field' => 'term_id',
            'terms' => array(33, 35),
            'include_children' => true,
            'operator' => 'IN'
        ),
        array(
            'taxonomy' => 'category', // standard category
            'field' => 'slug',
            'terms' => array('dogs'),
            'include_children' => true,
            'operator' => 'IN'
        )
    )
);
```

## Custom Filters

- here we're filtering with a form
- the form uses the GET method and passes the queries to the URL
- to capture the min price & max price value we open some PHP above our form:

```php
if($_GET['minprice'] && !empty($_GET['minprice'])) {
  $minprice = $_GET['minprice'];
}
if($_GET['maxprice'] && !empty($_GET['maxprice'])) {
  $maxprice = $_GET['maxprice'];
}
```

- we're saying if we have this value & its not empty set the value to a variable
- then in our query we can use those values

```php
$args = array(
    'post_type' => 'post',
    'posts_per_page' => -1,
    'meta_query' => array(
        array(
            'key' => 'price',
            'type' => 'NUMERIC',
            'value' => array($minprice, $maxprice),
            'compare' => 'BETWEEN'
        )
    )
);
```

- our full page with query and HTML

```php
<?php get_header(); ?>
    <?php
        if($_GET['minprice'] && !empty($_GET['minprice']))
        {
            $minprice = $_GET['minprice'];
        } else {
            $minprice = 0; // setting a fallback of 0 so BETWEEN works
        }

        if($_GET['maxprice'] && !empty($_GET['maxprice']))
        {
            $maxprice = $_GET['maxprice'];
        } else {
            $maxprice = 999999; // setting a fallback of 99999 so BETWEEN works
        }

        if($_GET['size'] && !empty($_GET['size']))
        {
            $size = $_GET['size'];
        }

        if($_GET['color'] && !empty($_GET['color']))
        {
            $color = $_GET['color'];
        }
    ?>


	<div class="container">
        <h4 class="lesson-title">WP Custom Query - Custom Filters</h4>
        <form action="/" method="get">
            <label>min:</label>
            // Adding the value here keeps the value we set in the field
            <input type="number" name="minprice" value="<?php echo $minprice; ?>">
            <label>max:</label>
            <input type="number" name="maxprice" value="<?php echo $maxprice; ?>">

            <label>Size:</label>
            <select name="size">
                <option value="">Any</option>
                <option value="s">S</option>
                <option value="m">M</option>
                <option value="l">L</option>
                <option value="xl">XL</option>
            </select>

            <label>Color:</label>
            <select name="color">
                <option value="">Any</option>
                <option value="red">Red</option>
                <option value="blue">Blue</option>
                <option value="green">Green</option>
                <option value="yellow">Yellow</option>
                <option value="purple">Purple</option>
                <option value="black">Black</option>
            </select>
            <button type="submit" name="">Filter</button>
        </form>
        <?php
                $args = array(
                    'post_type' => 'post',
                    'posts_per_page' => -1,
                    'meta_query' => array(
                        array(
                            'key' => 'price',
                            'type' => 'NUMERIC',
                            'value' => array($minprice, $maxprice),
                            'compare' => 'BETWEEN'
                        ),

                        array(
                            'key' => 'size',
                            'value' => $size,
                            'compare' => 'LIKE'
                        ),

                        array(
                            'key' => 'color',
                            'value' => $color,
                            'compare' => 'LIKE'
                        )
                    )
                );
                $query = new WP_Query($args);
                while($query -> have_posts()) : $query -> the_post();
        ?>
            <div class="post clearfix">
                <h5><?php the_title(); ?></h5>
                <div class="taxonomy clearfix">
                    <div class="price categories">
                        <strong>Price:</strong>
                        <?php the_field('price'); ?>
                    </div>

                    <div class="tags">
                        <strong>Color:</strong> <?php the_field('color'); ?><br>
                        <strong>Size:</strong> <?php the_field('size'); ?>
                    </div>
                </div>
            </div>
        <?php endwhile; wp_reset_query(); ?>
    </div>




<?php get_footer(); ?>
```

## Custom Search

- first we setup our search form

```html
<div class="search-site">
  <!-- action is set to the page URL we've setup for our results template -->
  <form action="/search" method="get">
    <input type="text" name="search_text" />
    <label>Type:</label>
    <select name="type">
      <option value="">Any</option>
      <option value="post">Posts</option>
      <option value="movies">Movies</option>
      <option value="books">Books</option>
    </select>
    <button type="submit" name="">Search</button>
  </form>
</div>
```

- this will add the results as query parameters to our URL
- now in our custom template we use those values

```php
<?php /* Template name: Custom Search */
    get_header();
?>

<?php
    if($_GET['search_text'] && !empty($_GET['search_text']))
    {
        $text = $_GET['search_text'];
    }

    if($_GET['type'] && !empty($_GET['type']))
    {
        $type = $_GET['type'];
    }

?>

<div class="container">
    <h4>Searching for: <?php echo $text; ?></h4>

    <?php
        $args = array(
            'post_type' => $type,
            'posts_per_page' => -1,
            's' => $text, // using WP Search variable
            /*'exact' => true,
            'sentence' => true*/
        );
        $query = new WP_Query($args);
        while($query -> have_posts()) : $query -> the_post();
    ?>
        <div class="post clearfix">
            <h5><?php the_title(); ?></h5>
            <strong>
                <?php if(get_post_type() == 'post'){ echo 'Post'; } ?>
                <?php if(get_post_type() == 'movies'){ echo 'Movie'; } ?>
                <?php if(get_post_type() == 'books'){ echo 'Books'; } ?>
            </strong>
        </div>
    <?php endwhile; wp_reset_query(); ?>
</div>


<?php get_footer(); ?>
```
