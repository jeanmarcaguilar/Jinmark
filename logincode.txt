<?php
include("connections.php");
$Email = $Password = '';
$EmailErr = $PasswordErr = '';

if($_SERVER["REQUEST_METHOD"]=="POST"){
    if(empty($_POST["Email"])){
        $EmailErr = "Email is required";
    } else {
        $Email = $_POST["Email"];
    }
    if(empty($_POST["Password"])){
        $PasswordErr = "Password is required";
    } else {
        $Password = $_POST["Password"];
    }

    if ($Email && $Password) {
        $query = "SELECT * FROM login_tbl WHERE Email = '$Email'";
        $result = mysqli_query($connections, $query);
        if ($result && mysqli_num_rows($result) > 0) {
            $row = mysqli_fetch_assoc($result);
            $dbPassword = $row["Password"];
            $dbAccountType = $row["Account_type"];

            if ($Password === $dbPassword) {
                if ($dbAccountType == "admin") {
                    header("Location:admin.php");
                    exit();
                } else {
                    header("Location:user.php");
                    exit();
                }
            } else {
                $PasswordErr = "Incorrect password";
            }
        } else {
            $EmailErr = "Email is not registered";
        }
    }
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Requirement Management</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <form id="loginForm" method="POST" action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]); ?>">
            <img src="https://res.cloudinary.com/dejzo3x6l/image/upload/v1462601844/login%20page%20design/3.png" alt="Profile-pic">
            <div class="input_box">
                <input type="text" name="Email" placeholder="Email" value="<?php echo htmlspecialchars($Email); ?>">
            </div>
            <div class="input_box">
                <input type="password" name="Password" placeholder="Password" id="password">
            </div>
            <div id="passwordError" style="color: red;"><?php echo $PasswordErr; ?></div>
            <div id="emailError" style="color: red;"><?php echo $EmailErr; ?></div>
            <input type="submit" name="submit" value="Login" class="submit_btn">
        </form>
    </div>
</body>
</html>