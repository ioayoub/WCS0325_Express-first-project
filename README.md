# Consigne

Cette consigne vous guidera à travers les étapes pour initialiser un projet Express.js, configurer un serveur, créer des routes, et organiser votre code de manière modulaire. Suivez chaque étape attentivement.

## Étape 1 : Créer un dossier pour le projet

- Créez un dossier pour votre projet, par exemple `mon-app-express`.
- Ouvrez ce dossier dans votre VSCode et ouvrez un terminal intégré.

## Étape 2 : Initialiser un projet Node.js avec npm

- Dans le terminal, exécutez la commande suivante pour initialiser un projet Node.js :

```bash
npm init
```

Cela créera un fichier `package.json` avec les configurations par défaut.
Prenez le temps d'analyser les informations qui y sont contenues.

## Étape 3 : Créer un fichier `.gitignore`

- Créez un fichier nommé `.gitignore` à la racine de votre dossier.
- Ajoutez-y les lignes suivantes pour ignorer les fichiers inutiles dans un contrôle de version (comme Git) :

```
node_modules
.env
```

Cela garantit que le dossier `node_modules` (qui contient les dépendances) ne sera pas versionné.

## Étape 4 : Installer Express.js

- Dans le terminal, exécutez la commande suivante pour installer Express.js comme dépendance de votre projet :

```bash
npm i express
```

Cela ajoutera Express.js à votre fichier `package.json` et installera les fichiers nécessaires dans `node_modules`.

## Étape 5 : Créer et lancer un serveur Express.js

- Créez un fichier nommé `index.js` à la racine de votre dossier.
- Ajoutez le code suivant dans `index.js` pour configurer un serveur Express de base :

```js
import express from "express";

const app = express();

const port = 3310;

app.listen(port, () => {
  console.log(`Server is running on ${port} 🚀`);
});
```

- Lancez le serveur avec la commande :

```bash
node index.js
```

Vous devriez voir le message `SServer is running on 3310 🚀` dans le terminal.

## Étape 6 : Créer une route dans le fichier principal

- Dans `app.js`, ajoutez une route simple pour tester le serveur. Ajoutez ce code avant `app.listen` :

```js
app.get(’/’, (req, res) => {
    res.json({
        message: "Welcome to Express.js"
    });
});
```

- Redémarrez le serveur avec `node app.js`, puis ouvrez votre client HTTP (Postman, Insomnia...) à l’adresse `http://localhost:3000`. Vous devriez voir le message affiché.

- Redémarrez le serveur avec `node app.js`, puis ouvrez votre navigateur à l’adresse `http://localhost:3000`. Vous devriez voir le message affiché.

## Étape 7 : Déplacer la route dans un fichier `router.js`

- Créez un fichier nommé `router.js` dans le dossier `src`.
- Déplacez la logique de la route de `index.js` vers `router.js` avec le code suivant :

```bash
npm i dotenv
```

Cela permettra de charger des variables depuis un fichier `.env`.

- Créez un fichier nommé `.env` à la racine de votre projet et ajoutez-y la configuration du port :

```
APP_PORT=3310
```

- Modifiez `index.js` pour charger les variables d’environnement avec `dotenv` et utiliser le port défini dans `.env`. Ajoutez ceci en haut du fichier :

```js
require(‘dotenv’).config();

const port = process.env.PORT;
```

Votre fichier `index.js`devra donc ressembler à ceci:

```js
import express from "express";
require(‘dotenv’).config()

const app = express();

const port =  process.env.PORT;

//... votre route

app.listen(port, () => {
    console.log(`Server is running on ${port} 🚀`);
});
```

Assurez-vous que la ligne `app.listen` utilise bien `port` comme variable.

- Relancez le serveur avec :

```bash
node app.js
```

Vérifiez que le serveur fonctionne toujours sur `http://localhost:3310`.

## Étape 8 : Automatiser le redémarrage du serveur avec un script `dev`

- Pour éviter de relancer manuellement le serveur à chaque modification, utilisez le mode watch de Node.js (disponible depuis la version 18.11.0).
- Ajoutez un script `dev` dans votre fichier `package.json`.
- Ouvrez `package.json` et modifiez la section `scripts` comme suit :
-

```
“scripts”: {
    “dev”: “node –watch index.js”
}
```

Cela configure un script pour exécuter votre application en mode watch, qui redémarre automatiquement le serveur à chaque changement de fichier.

- Lancez le serveur en mode développement avec la commande :

```
npm run dev
```

Testez en modifiant un fichier (par exemple, le message dans la route de `index.js` ou un `console.log`), et vérifiez que le serveur redémarre automatiquement sans intervention manuelle.

## Étape 9 : Déplacer la route dans un fichier `router.js`

- Créez un fichier nommé `router.js`.
- Déplacez la logique de la route de `index.js` vers `router.js` avec le code suivant :

```js
import express from "express";

const router = express.Router();

router.get(’/’, (req, res) => {
     res.json({
        message: "Welcome to Express.js"
    });
});

export default router;
```

Cela définit un routeur modulaire pour gérer les routes.

## Étape 10 : Importer et utiliser le routeur dans `index.js`

- Dans `app.js`, supprimez la définition de la route et remplacez-la par l’importation du routeur. Modifiez `index.js` comme suit :

```js
const express = require(‘express’);
require(‘dotenv’).config();

const app = express();

const port = process.env.PORT;

import mainRouter from "./router.js"

app.use(mainRouter);

app.listen(port, () => {
    console.log(`Serveur démarré sur le port ${port}`);
});
```

- Redémarrez le serveur avec `npm run dev` et vérifiez que la route fonctionne toujours à `http://localhost:3310`.

## Étape 11 : Déplacer la logique de la route dans un fichier `totoActions.js`

- Créez un dossier nommé `modules` dans le dossier `src`.
- Dans ce dossier, créez un dossier `toto`puis un fichier nommé `totoActions.js`.
- Déplacez la logique de la réponse de la route dans ce fichier avec le code suivant :
-

```js
const sayHello = (req, res) => {
  res.json({
    message: "Welcome to Express.js",
  });
};

export default { sayHello };
```

- Modifiez votre fichier `router.js` pour importer et utiliser cette logique :

```js
import express from "express";

const router = express.Router();

import totoActions from "./toto/totoActions.js"

router.get(’/’, totoActions.sayHello);

export default router;
```

- Redémarrez le serveur avec `npm run dev` et vérifiez que tout fonctionne encore à `http://localhost:3310`.

## Résultat attendu

À la fin de ces étapes, vous aurez une application Express.js organisée avec :

- Un serveur principal dans `index.js` utilisant des variables d’environnement via `dotenv`.
- Un script `dev` dans `package.json` pour un redémarrage automatique avec le mode watch de Node.js.
- Des routes définies dans `router.js`.
- La logique des routes dans `modules/toto/totoActions.js`.
- Un projet prêt à être étendu avec d’autres routes et fonctionnalités.

N’hésitez pas à tester chaque étape dans votre navigateur ou avec des outils comme Postman pour vérifier que les routes répondent correctement. Si vous rencontrez des erreurs, vérifiez les messages dans le terminal pour identifier le problème.
