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

<center>![]({{ site.baseurl }}/images/03.png)</center>



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


<!-- 

# Publier mon composant inputsur la plateforme NPM 
Afin de publier votre nouveau composant sur NPM, il est nécessaire de créer un compte au préalable sur la [plateforme](https://www.npmjs.com).
 -->
