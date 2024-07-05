Install the theme into the themes directory and activate it through the UI, or wp-cli

make a child theme of the parent theme.
If a third party child theme, then you can minimize the edits needed to that theme to make your own
child theme by following the instructions at
https://stackoverflow.com/questions/46913744/wordpress-child-theme-of-a-child-theme

Create a theme template that will be used for the specific page using bootstrap

Check that the page is using that template before enqueueing the scripts
https://stackoverflow.com/questions/52802380/how-to-enqueue-the-bootstrap-script-and-style-for-wordpress-page-template-but-no

Enqueue the bootstrap scripts
```
<!-- Latest compiled and minified CSS -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">

<!-- jQuery library -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.7.1/jquery.min.js"></script>

<!-- Latest compiled JavaScript -->
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
```





