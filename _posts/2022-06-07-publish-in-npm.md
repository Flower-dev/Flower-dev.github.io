---
layout: post
title:  "Publier un composant React JS sur NPM"
date:   2022-06-07 18:05:55 +0300
image:  02.jpg
tags:   NPM
---

Envie de créer un composant réutilisable en React JS et de le publier sur NPM ? Je vous explique comment faire tout cela en créant un composant input assez simple 😉

# Créer un composant Input réutilisable avec React JS & le publier sur NPM

## Etape 1 : Création d'un nouveau repository Github & installation d'un nouveau projet React


Première étape de ce tuto, commencez par créer un nouveau repository Github et clonez le afin de pouvoir l'utiliser. 

Une fois cette étape réalisée, vous pouvez créer un nouveau projet React en lançant la commande suivante :

```powershell
$ npx create-react-app input_plugin
```


## Etape 2 : Création du composant input 


Pour commencer, supprimez tous les fichiers du dossier `src` et remplacez les par un nouveau dossier `lib` dans lequel vous mettrez l'ensemble des fichiers relatif au composant Input. Vous devriez avoir cela dans votre IDE.


![]({{ site.baseurl }}/images/03.png)


Dans ce nouveau dossier franchement créé, ajoutez un fichier `Input.js` dans lequel vous pouvez écrire le code suivant : 


```js
import { useState } from 'react';
import PropTypes from 'prop-types';

Input.propTypes = {
    type: PropTypes.string,
    name: PropTypes.string,
    placeholder: PropTypes.string,
    onChange: PropTypes.func,
    required: PropTypes.bool,
    min: PropTypes.string
}

Input.defaultProps = {
    required: false,
    min: '0'
}

export function Input({ type, name, placeholder, onChange, required, min }) {
    
    const [initialValue, setInitialValue] = useState('');

    return  (
        <input 
            className='inputcustom' 
            type={type}
            name={name}
            required={required}
            placeholder={placeholder}
            min={min} 
            value={initialValue}   
            onChange={(e) => {
                setInitialValue(e.target.value)
                onChange(e.target.value)
            }} 
        />
    )
}
```


Vous pouvez lui ajouter du style en créant un  nouveau fichier `input.css` avec le code suivant : 


```css
.inputcustom {
    background-color: #eee;
    border: none;
    border-radius: 5px;
    padding: 12px 15px;
    margin: 8px 0;
    width: 94%;
}
```

et l'importer dans le fichier `Input.js` 


```js
import './input.css';
```


Pour terminer avec le dossier `lib`, créez un dernier fichier `index.js` dans lequel vous ajouterez le code suivant 


```js
export {Input} from './Input';
```


Cela vous permettra d'utiliser votre composant le moment venu. 😉


## Etape 3 : Mise à jour du fichier package.json et gestion des dépendances


Dans le fichier `package.json`, commencez par déplacer les dépendances se trouvant dans `dependencies`


```json
"dependencies": {
    // .....
    "react": "^18.1.0",
    "react-dom": "^18.1.0"
    // .......
}
```

dans un nouvel objet `peerDependencies` comme ci-dessous : 


```json
"peerDependencies": {
    "react": "^18.1.0",
    "react-dom": "^18.1.0"
},
```


Cela permet d'éviter les conflits de DOM entre le composant React que vous êtes en train de créer et le projet React dans lequel il sera importé par la suite.

Vous devriez vous retrouver avec le fichier suivant : 


```json
{
    "name": "input_plugin",
    "version": "0.1.0",
    "private": true,
    "peerDependencies": {
        "react": "^18.1.0",
        "react-dom": "^18.1.0"
    },
    "dependencies": {
        "@testing-library/jest-dom": "^5.16.4",
        "@testing-library/react": "^13.3.0",
        "@testing-library/user-event": "^13.5.0",
        "react-scripts": "5.0.1",
        "web-vitals": "^2.1.4"
    },
    "scripts": {
        "start": "react-scripts start",
        "build": "react-scripts build",
        "test": "react-scripts test",
        "eject": "react-scripts eject"
    },
    "eslintConfig": {
        "extends": [
            "react-app",
            "react-app/jest"
        ]
    },
    "browserslist": {
        "production": [
            ">0.2%",
            "not dead",
            "not op_mini all"
        ],
        "development": [
            "last 1 chrome version",
            "last 1 firefox version",
            "last 1 safari version"
        ]
    }
}
```


Maintenant, installez Babel en excutant les commandes suivantes : 


```bash
npm install --save-dev @babel/core @babel/cli @babel/preset-env 
npm install --save @babel/polyfill
```


Une fois les dépendances Babel installées dans votre fichier `package.json`, créez un nouveau dossier `babel.config.json` à la racine de votre projet et ajoutez lui le code suivant : 


```json
{
    "presets": [
        [
            "@babel/env",
            {
                "targets": {
                    "edge": "17",
                    "firefox": "60",
                    "chrome": "67",
                    "safari": "11.1"
                },
                "useBuiltIns": "usage",
                "corejs": "3.6.5"
            }
        ],
        "@babel/preset-react"
    ]
}
```


Dans le fichier `package.json` remplacez la ligne  


```json
"build": "react-scripts build",
``` 

par ce qui suit : 


```json
"build": "rm -rf dist && NODE_ENV=production babel src/lib --out-dir dist --copy-files",
```

et lancer la commande 


```bash
npm run build
```


dans votre terminal, ce qui créer un dossier `dist` comprenant une copie des fichiers `Input.js`, `index.js` et `input.css` créés au paravant. 

Avant de passer à l'étape de publication, vous devriez vous retrouver avec l'ensemble de ces dossiers et fichiers dans votre éditeur de code préféré 🗒

![]({{ site.baseurl }}/images/04.png)


# Publier mon composant inputsur la plateforme NPM 
Afin de publier votre nouveau composant sur NPM, il est nécessaire d'avoir un compte NPM, si vous n'en avez pas vous pouvez vous en créer un directement sur la [plateforme](https://www.npmjs.com).


Une fois votre compte créé, vous devez vous y connecterà partir de votre terminal en tapant la commande suivante : 


```bash
$ npm login 
```


Dans votre fichier `package.json` supprimez la ligne `"private:" true` et lancer la commande 


```bash
$ npm init
```


pour mettre à jour votre manifeste avec l'ensemble des informations nécessaires au bon fonctionnement de votre composant. Enfin, lancer la commande suivante dans votre terminal


```bash
$ npm publish
```


afin de publier votre composant sur NPM 

⚠️  Attention, le nom de votre composant doit être unique et le registre NPM est sensible à la casse, si jamais votre paquet rentre en conflit avec un paquet déjà existant, votre terminal vous renverra l'erreur suivante :


```bash
403 Forbidden - PUT https://registry.npmjs.org/[package] - You do not have permission to publish "[package]". Are you logged in as the correct user?
```


vous devez donc changer le nom du plugin que vous êtes en train de créer dans le manifeste pour évider un éventuel conflit 😉

Une fois publié sur NPM, votre composant est prêt à être utilisé !!!


Vous pouvez retrouver la codeBase du projet en cliquant [ici](https://github.com/Flower-dev/input_plugin)

