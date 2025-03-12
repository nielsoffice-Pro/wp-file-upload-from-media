# wp-file-upload-from-media
File upload from media WordPress

```PHP
<?php 

function vas_dev() { 
  
    echo do_shortcode('[custom_file_upload_form]'); 
  
    
 }

// Enqueue WordPress media scripts
function enqueue_media_uploader() {
    if (is_admin()) {
        wp_enqueue_media();
    }
}
add_action('admin_enqueue_scripts', 'enqueue_media_uploader');


// Output the form with the file input and button
function custom_file_upload_form() {
    ?>
    <form id="custom-upload-form" method="post" enctype="multipart/form-data">
        <input type="text" name="file_url" id="file_url" readonly />
        <button type="button" id="upload-button">Select File from Media Library</button>
        <input type="submit" name="submit" value="Upload" />
    </form>

    <script type="text/javascript">
        jQuery(document).ready(function($){
            $('#upload-button').click(function(e) {
                e.preventDefault();
                var custom_uploader = wp.media({
                    title: 'Select File',
                    button: {
                        text: 'Use this file'
                    },
                    multiple: false
                })
                .on('select', function() {
                    var attachment = custom_uploader.state().get('selection').first().toJSON();
                    $('#file_url').val(attachment.url);
                })
                .open();
            });
        });
    </script>
    
    <?php } 
    
    add_shortcode('custom_file_upload_form', 'custom_file_upload_form'); 
    
    // Handle the file upload or selection 
    function handle_custom_file_upload() {
    if (isset($_POST['submit'])) {
        // Get the file URL from the input
        $file_url = $_POST['file_url'];

        // If a file URL is provided, process it
        if (!empty($file_url)) {
            echo "File selected: " . esc_url($file_url);
            // You can add more processing code here if needed
        } else {
            echo "No file selected.";
        }
    } 
  }  add_action('init', 'handle_custom_file_upload');

?>

```

<h5>Make Media file available by user upload or simply cannot view other upload from otehr users</h5>

```PHP

 // Make media available only bu user upload
 add_action( 'pre_get_posts', 'restrict_media_library_to_current_user' );
 function restrict_media_library_to_current_user( $query ) {
    // Check if we're on the media library page and the user is not an administrator
    if ( is_admin() && isset( $query->query_vars['post_type'] ) && 'attachment' === $query->query_vars['post_type'] ) {
        // Get the current user ID
        $current_user_id = get_current_user_id();

        // Restrict media to only files uploaded by the current user
        $query->set( 'author', $current_user_id );
    }
}

```
