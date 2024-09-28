# checkpont-express



Étape 1: Installer les dépendances
Assurez-vous d'avoir Node.js installé, puis créez un nouveau projet et installez Express :

mkdir mon-application  
cd mon-application  
npm init -y  
npm install express  
Étape 2: Créer la structure du projet
Créez les fichiers et dossiers suivants :

/mon-application  
|-- /public  
|   |-- styles.css  
|-- /views  
|   |-- index.ejs  
|   |-- services.ejs  
|   |-- contact.ejs  
|-- app.js  
Étape 3: Écrire le serveur Express (app.js)
const express = require('express');  
const app = express();  
const path = require('path');  

// Middleware pour servir les fichiers statiques  
app.use(express.static('public'));  

// Middleware personnalisé pour vérifier les heures de travail  
const checkWorkingHours = (req, res, next) => {  
    const now = new Date();  
    const day = now.getDay(); // 0 = dimanche, 1 = lundi, ..., 6 = samedi  
    const hour = now.getHours();  
    const isWeekend = day === 0 || day === 6;  
    const isWorkingHours = hour >= 9 && hour < 17;  

    if (isWeekend || !isWorkingHours) {  
        return res.status(403).send('L\'application est disponible uniquement pendant les heures de travail (du lundi au vendredi, de 9 à 17).');  
    }  
    next();  
};  

// Appliquer le middleware à toutes les routes  
app.use(checkWorkingHours);  

// Configurer le moteur de modèles EJS  
app.set('view engine', 'ejs');  
app.set('views', path.join(__dirname, 'views'));  

// Routes  
app.get('/', (req, res) => {  
    res.render('index');  
});  

app.get('/services', (req, res) => {  
    res.render('services');  
});  

app.get('/contact', (req, res) => {  
    res.render('contact');  
});  

// Démarrer le serveur  
const PORT = process.env.PORT || 3000;  
app.listen(PORT, () => {  
    console.log(`Le serveur fonctionne sur http://localhost:${PORT}`);  
});  
Étape 4: Créer les vues EJS
views/index.ejs
<!DOCTYPE html>  
<html lang="fr">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>Page d'accueil</title>  
    <link rel="stylesheet" href="/styles.css">  
</head>  
<body>  
    <nav>  
        <a href="/">Accueil</a>  
        <a href="/services">Nos services</a>  
        <a href="/contact">Nous contacter</a>  
    </nav>  
    <h1>Bienvenue sur notre site web</h1>  
    <p>Ceci est la page d'accueil.</p>  
</body>  
</html>  
views/services.ejs
<!DOCTYPE html>  
<html lang="fr">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>Nos Services</title>  
    <link rel="stylesheet" href="/styles.css">  
</head>  
<body>  
    <nav>  
        <a href="/">Accueil</a>  
        <a href="/services">Nos services</a>  
        <a href="/contact">Nous contacter</a>  
    </nav>  
    <h1>Nos Services</h1>  
    <p>Voici les services que nous proposons.</p>  
</body>  
</html>  
views/contact.ejs
<!DOCTYPE html>  
<html lang="fr">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>Nous Contacter</title>  
    <link rel="stylesheet" href="/styles.css">  
</head>  
<body>  
    <nav>  
        <a href="/">Accueil</a>  
        <a href="/services">Nos services</a>  
        <a href="/contact">Nous contacter</a>  
    </nav>  
    <h1>Nous Contacter</h1>  
    <p>Vous pouvez nous contacter par email à contact@example.com.</p>  
</body>  
</html>  
Étape 5: Ajouter le style CSS (public/styles.css)
body {  
    font-family: Arial, sans-serif;  
    background-color: #f4f4f4;  
    margin: 0;  
    padding: 0;  
}  

nav {  
    background-color: #333;  
    padding: 10px;  
}  

nav a {  
    color: white;  
    padding: 14px 20px;  
    text-decoration: none;  
}  

nav a:hover {  
    background-color: #575757;  
}  

h1 {  
    color: #333;  
}  
Étape 6: Démarrer le serveur
Dans le terminal, exécutez la commande suivante :

node app.js  
