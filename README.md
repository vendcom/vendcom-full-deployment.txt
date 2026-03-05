# vendcom-full-deployment.txt
vendcom
## vendcom/frontend/index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vendcom - Landing Page</title>
    <link rel="stylesheet" href="css/styles.css">
</head>
<body>
    <header>
        <h1>Vendcom Compliance System</h1>
    </header>

    <main>
        <section class="hero">
            <img src="../assets/images/laptop.png" alt="Laptop">
            <div class="pricing-boxes">
                <div class="pricing-box">Basic</div>
                <div class="pricing-box">Pro</div>
                <div class="pricing-box">Enterprise</div>
            </div>
        </section>
    </main>

    <footer>
        <div class="bottom-tab">Demo</div>
    </footer>

    <script src="js/scripts.js"></script>
</body>
</html>

## vendcom/frontend/login.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vendcom Login</title>
    <link rel="stylesheet" href="css/styles.css">
</head>
<body>
    <h2>Login to Vendcom</h2>
    <form action="../backend/server.php" method="POST">
        <label>Username:</label>
        <input type="text" name="username" required>
        <label>Password:</label>
        <input type="password" name="password" required>
        <button type="submit" name="login">Login</button>
    </form>
</body>
</html>

## vendcom/frontend/dashboard.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vendcom Dashboard</title>
    <link rel="stylesheet" href="css/styles.css">
</head>
<body>
    <h2>User Dashboard</h2>
    <p>Welcome, demo user!</p>
    <a href="login.html">Logout</a>
</body>
</html>

## vendcom/frontend/admin.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vendcom Admin Panel</title>
    <link rel="stylesheet" href="css/styles.css">
</head>
<body>
    <h2>Admin Panel</h2>
    <p>Manage users, pricing, and settings</p>
</body>
</html>

## vendcom/frontend/css/styles.css
body { font-family: Arial, sans-serif; margin: 0; padding: 0; }
header, footer { background: #222; color: #fff; padding: 10px; text-align: center; }
.pricing-boxes { display: flex; justify-content: center; gap: 10px; margin-top: 20px; }
.pricing-box { border: 1px solid #ccc; padding: 20px; width: 100px; text-align: center; }
.bottom-tab { background: #444; color: #fff; padding: 10px; text-align: center; margin-top: 20px; }
form { max-width: 300px; margin: 20px auto; display: flex; flex-direction: column; gap: 10px; }
input, button { padding: 10px; }

## vendcom/frontend/js/scripts.js
console.log("Vendcom frontend loaded");

## vendcom/backend/server.php
<?php
session_start();
include 'config.php';
include 'utils.php';

if(isset($_POST['login'])){
    $username = $_POST['username'];
    $password = $_POST['password'];

    $stmt = $conn->prepare("SELECT * FROM users WHERE username=?");
    $stmt->bind_param("s",$username);
    $stmt->execute();
    $result = $stmt->get_result();

    if($result->num_rows == 1){
        $user = $result->fetch_assoc();
        if(password_verify($password,$user['password'])){
            $_SESSION['user'] = $user['username'];
            header("Location: ../frontend/dashboard.html");
        } else {
            echo "Invalid password";
        }
    } else {
        echo "User not found";
    }
}
?>

## vendcom/backend/config.php
<?php
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "vendcom_db";

$conn = new mysqli($servername, $username, $password, $dbname);
if($conn->connect_error){
    die("Connection failed: " . $conn->connect_error);
}
?>

## vendcom/backend/utils.php
<?php
function sanitize($data){
    return htmlspecialchars(stripslashes(trim($data)));
}
?>

## vendcom/database/vendcom_demo.sql
CREATE DATABASE IF NOT EXISTS vendcom_db;
USE vendcom_db;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL
);

INSERT INTO users (username, password) VALUES 
('demo_user', '$2y$10$wH1kK9Y0.kHxqHn5aY2oMea2kZYqgjDnx3JjvkkkWhQH5nQw92tJ6');
-- Password is "demo123"

## vendcom/assets/images/laptop.png
[placeholder for laptop image]

## vendcom/assets/logos/
[add your logos here]

## vendcom/assets/icons/
[add your UI icons here]

## vendcom/instructions/IONOS_deployment_guide.txt
1. Upload the entire Vendcom folder to your IONOS hosting server.  
2. Create a MySQL database and import `database/vendcom_demo.sql`.  
3. Open `backend/config.php` and update `$username`, `$password`, `$dbname` with your IONOS credentials.  
4. Ensure the `frontend` folder is in your web root.  
5. Test login:  
   Username: demo_user  
   Password: demo123  
6. Verify landing page, dashboard, and admin panel.  
7. Point your domain to the server root.
