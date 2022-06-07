---
layout: post
title:  "Publier un composant React JS sur NPM"
date:   2022-06-07 18:05:55 +0300
image:  02.jpg
tags:   Dev
---

Envie de créer un composant réutilisable en React JS et de le publier sur NPM ? Je vous explique comment faire tout ça en créant un composant input assez simple 😉

# Créer mon premier composant input réutilisable React JS

## Etape 1 : Création nouveau repository Github et création d'un nouveau projet React

Première étape de ce tuto, commencez par créer un nouveau repository Github et cloné afin de pouvoir l'utiliser. 

Une fois le repository cloné sur votre ordinateur, vous pouvez créer un nouveau projet React en lançant la commande suivante : 
```bash
npx create-react-app input_plugin
```


## Etape 2 : Création du composant input 

Dans le dossier `src`, supprimez tous les fichiers déjà existant et créer un nouveau nouveau dossier `lib`

![]({{ site.baseurl }}/images/03.png)

Dans ce nouveau dossier `lib`, créez un nouveau fichier `Input.js` dans lequel vous pouvez écrire le code suivant : 


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

export default function Input({ type, name, placeholder, onChange, required, min }) {
    
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

Une fois le composant input créé, vous pouvez lui ajouter du style en ajoutant un fichier `input.css` avec le code suivant : 

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

et l'importer dans `input.js` 

```js
import './input.css';
```

Pour terminer avec le dossier `lib`, créez un dernier fichier `index.js` dans lequel vous ajouterez le code suivant 

```js
export {Input} from './Input';
```

## Etape 3 : Mise à jour du fichier package.json et gestion des dépendances

Dans le fichier `package.json`, commencez par déplacer les dépendances 

```json
"dependencies": {
    // .....
    "react": "^18.1.0",
    "react-dom": "^18.1.0"
    // .......
}
```

dans 

```json
"peerDependencies": {
    "react": "^18.1.0",
    "react-dom": "^18.1.0"
},
```

Vous devez vous retrouvez avec le fichier suivant : 

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
npm install -save @babel/polyfill
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

dans votre terminal, ce qui créer un dossier `dist` comprenant une copie des fchiers `Input.js`, `index.js` et `input.css` que nous avions créé au paravant. 

Avant de passer à l'étape de publication, vous devriez vous retrouver avec l'ensemble de ces dossiers et fichiers dans votre éditeur de code préféré 🗒

![]({{ site.baseurl }}/images/04.png)

<!-- 

# Publier mon composant inputsur la plateforme NPM 
Afin de publier votre nouveau composant sur NPM, il est nécessaire de créer un compte au préalable sur la [plateforme](https://www.npmjs.com).
 -->
