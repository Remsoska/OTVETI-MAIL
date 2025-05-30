стили. body {

    font-family: Arial, sans-serif;

    max-width: 800px;

    margin: 0 auto;

    padding: 20px;

}

.form-group {

    margin-bottom: 15px;

}

.form-group label {

    display: block;

    margin-bottom: 5px;

}

.form-group input, .form-group select, .form-group textarea {

    width: 100%;

    padding: 8px;

    box-sizing: border-box;

}

.btn {

    display: inline-block;

    padding: 8px 15px;

    background: #3498db;

    color: white;

    text-decoration: none;

    border: none;

    cursor: pointer;

}

.alert {

    padding: 10px;

    margin-bottom: 15px;

}

table {

    width: 100%;

    border-collapse: collapse;

    margin-bottom: 20px;

}

table th, table td {

    padding: 8px;

    border: 1px solid #ddd;

}






ню_букинг. <?php

require 'functions.php';

check_auth();

if ($_SERVER['REQUEST_METHOD'] == 'POST') {

    $date = $_POST['date'];

    $time = $_POST['time'];

    $guests = (int)$_POST['guests'];


    $datetime = date('Y-m-d H:i:s', strtotime("$date $time"));


    $stmt = $pdo->prepare("INSERT INTO bookings (user_id, date, guests) VALUES (?, ?, ?)");

    $stmt->execute([$_SESSION['user_id'], $datetime, $guests]);


    header("Location: bookings.php");

    exit;

}

?>

<!DOCTYPE html>

<html>

<head>

    <title>Новое бронирование</title>

    <link rel="stylesheet" href="style.css">

</head>

<body>

    <h1>Новое бронирование</h1>

    <form method="POST">

        <div class="form-group">

            <label>Дата:</label>

            <input type="date" name="date" required min="<?= date('Y-m-d') ?>">

        </div>

        <div class="form-group">

            <label>Время:</label>

            <select name="time" required>

                <?= get_times() ?>

            </select>

        </div>

        <div class="form-group">

            <label>Количество гостей:</label>

            <select name="guests" required>

                <?php for ($i = 1; $i <= 10; $i++): ?>

                <option value="<?= $i ?>"><?= $i ?></option>

                <?php endfor; ?>

            </select>

        </div>

        <button type="submit" class="btn">Забронировать</button>

    </form>

</body>

</html>






индекс. <?php require 'functions.php'; ?>

<!DOCTYPE html>

<html>

<head>

    <title>Главная</title>

    <link rel="stylesheet" href="style.css">

</head>

<body>

    <h1>Я буду кушац</h1>

    <p><a href="login.php" class="btn">Вход</a> <a href="register.php" class="btn">Регистрация</a></p>

</body>

</html>











функтионс. <?php

require 'config.php';

// Проверка авторизации пользователя

function check_auth() {

    if (!isset($_SESSION['user_id'])) {

        header("Location: login.php");

        exit;

    }

}

// Проверка администратора

function check_admin() {

    if (!isset($_SESSION['admin'])) {

        header("Location: login.php");

        exit;

    }

}

// Генерация времени для бронирования

function get_times() {

    $times = '';

    for ($h = 10; $h <= 22; $h++) {

        $times .= "<option value='$h:00'>$h:00</option>";

        if ($h != 22) $times .= "<option value='$h:30'>$h:30</option>";

    }

    return $times;

}

?>
