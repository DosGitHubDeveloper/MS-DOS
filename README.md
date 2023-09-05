# MS-DOS
Simple DOS application in your web browser
NOTICE: This is currently in Alpha testing so it may be a little buggy.
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <title>Testform for a DOS-like behavior</title>
</head>
<body>
<pre>
<?php print_r($_POST); ?>
</pre>
<form action="testform.php" method="post" id="formmail">
    <fieldset id="person">
        <legend>Person</legend>
        <label for="name">Name:</label>
        <input type="text" name="name" id="name" value="">
        <br>
        <label for="email">E-Mail:</label>
        <input type="text" name="email" id="email" value="">
        <br>
        <label for="phone">Phone:</label>
        <input type="text" name="phone" id="phone" value="">
    </fieldset>
    <fieldset id="productgroup1">
        <legend>Product Group 1</legend>
        Product: <input type="text" name="product[1][]">
        Qty: <input type="text" name="qty[1][]">
    </fieldset>
    <input class="button" type="submit" value="Submit">
</form>
</body>
</html>
