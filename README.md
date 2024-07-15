# wp-file-upload-from-media
File upload from media WordPress

```
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
