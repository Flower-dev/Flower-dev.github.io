---
layout: post
title:  "Les Hooks"
date:   2023-03-10 18:05:55 +0300
image:  13.png
tags:   JS
---


# React et ses hooks


Les hooks, introduits avec React 16.8, sont des fonctions permettant de manipuler les états et les cycles de vie des composants React de manière simple et efficace dans le but de les rendre  plus modulaires, réutilisables et faciles à tester.

Il existe plusieurs types de hooks React JS qui ont chacun une fonctionnalité spécifique. Dans cet article, nous allons passer en revue les sept hooks les plus couramment utilisés avec des exemples d'utilisation.

## useState
Le hook `useState` permet de définir et  mettre à jour l'état local d'un composant. Il prend une valeur initiale en paramètre et renvoie un tableau contenant l'état actuel et une fonction pour le mettre à jour.

Voici un exemple :

```js
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

Dans cet exemple, `useState` est utilisé pour définir et mettre à jour l'état local count. La fonction `setCount` permet mettre à jour l'état lorsque l'on clique sur le bouton : `onClick={() => setCount(count + 1)}`.

## useEffect

Le hook `useEffect` permet  quant à lui de gérer les effets de bord d'un composant. Il est utilisé pour effectuer des actions après le render d'un composant, telles que l'envoi de requêtes HTTP ou la modification du DOM.


```js
import React, { useState, useEffect } from 'react';

function User(props) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetch(`https://jsonplaceholder.typicode.com/users/${props.id}`)
      .then(response => response.json())
      .then(data => setUser(data));
  }, [props.id]);

  if (!user) {
    return <div>Loading...</div>;
  }

  return (
    <div>
      <p>Name: {user.name}</p>
      <p>Email: {user.email}</p>
    </div>
  );
}
```

Dans cet exemple, `useEffect` est utilisé pour envoyer une requête HTTP dans le but de récupérer les données d'un utilisateur en fonction de l'ID passé en paramètre. Afin de pouvoir faire cet appel au bon moment, 'useEffext' prend en deuxième argument une liste de dépendances, `[props.id]`, indiquant à quel moment l'effet doit être déclenché.


## useContext
Le hook `useContext` permet d'accéder à un contexte React à partir d'un composant. Il est utilisé pour éviter de passer des propriétés de composant en composant, rendant le code plus lisible et plus facile à maintenir.

Exemple :

```js
import React, { useContext } from 'react';

const UserContext = React.createContext();

function User(props) {
  const user = useContext(UserContext);

  return (
    <div>
      <p>Name: {user.name}</p>
      <p>Email: {user.email}</p>
    </div>
  );
}

function App() {
  const user = { name: 'John Doe', email: 'john@example.com' };

  return (
    <UserContext.Provider value={user}>
      <User />
    </UserContext.Provider>
  );
}
```
Dans cet exemple, `useContext` permet accéder au contexte `UserContext` créé dans le composant parent `App`. Il est ainsi possible d'obtenir l'objet `user` sans avoir à le passer en tant que propriété de composant à chaque fois.

## useReducer
Le hook `useReducer` permet de gérer les états complexes à l'aide d'un reducer. Il est utilisé pour séparer la logique de mise à jour de l'état de la logique de rendu du composant.

Exemple :

```js
import React, { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
    </div>
  );
}
```

Dans cet exemple, `useReducer` sert à gérer l'état du composant `Counter`. Pour cela, il faut définir un état initial ainsi qu'un reducer déterminant comment l'état doit être mis à jour en fonction de l'action donnée. La fonction `dispatch` est utilisée quant à elle pour envoyer une action à notre reducer, qui mettra à jour l'état en conséquence.

## useCallback
Le hook `useCallback` permet de mémoriser une fonction pour éviter qu'elle ne soit recréée à chaque rendu du composant améliorant ainsi ses performances en évitant les recalculs inutiles.

Exemple :
```js
import React, { useState, useCallback } from 'react';

function MemoizedList(props) {
  const [selected, setSelected] = useState(null);

  const handleSelect = useCallback(
    (id) => {
      setSelected(id);
      props.onSelect(id);
    },
    [props.onSelect]
  );

  return (
    <ul>
      {props.items.map(item => (
        <li
          key={item.id}
          onClick={() => handleSelect(item.id)}
          style={selected === item.id ? { fontWeight: 'bold' } : {}}
        >
          {item.name}
        </li>
      ))}
    </ul>
  );
}
```

Dans cet exemple, `useCallback` permet de mémoriser la fonction `handleSelect`, qui est passée en tant que propriété de composant à `MemoizedList`. Cela permet d'éviter que la fonction ne soit recréée à chaque rendu de ce dernier.


## useMemo
Le hook `useMemo` permet de mémoriser une valeur calculée pour éviter qu'elle ne soit recalculée à chaque rendu du composant. Tout comme le hook précédent, `useMemo` aide à l'amélioration des performances en évitant les recalculs inutiles.

Exemple :
```js
import React, { useMemo } from 'react';

function ExpensiveComponent(props) {
  const result = useMemo(() => {
    return props.values.reduce((acc, val) => acc + val, 0);
  }, [props.values]);

  return <p>Result: {result}</p>;
}
```

Dans cet exemple, `useMemo` permet de mémoriser la valeur calculée `result`  dépendant des propriétés de composant `props.values`. Cette valeur est calculée une seule fois et est mémorisée pour éviter qu'elle ne soit recalculée à chaque rendu du composant.

## useRef
Le hook `useRef` permet de créer une référence mutable pouvant être utilisée pour stocker des valeurs qui ne déclenchent pas de nouveau rendu.

Exemple :
```js
import React, { useState, useRef } from 'react';

function InputWithButton(props) {
  const inputRef = useRef(null);

  const handleClick = () => {
    props.onSubmit(inputRef.current.value);
    inputRef.current.value = '';
  };

  return (
    <div>
      <input type="text" ref={inputRef} />
      <button onClick={handleClick}>Submit</button>
    </div>
  );
}
```

Dans cet exemple, `useRef` crée une référence mutable `inputRef`, qui est associée à l'élément `input`. Il suffit ensuite d'utiliser cette référence dans la fonction `handleClick` pour récupérer la valeur de l'élément `input` et réinitialiser sa valeur après la soumission.

Conclusion

Les hooks sont une des fonctionnalités clefs de React permettant d'écrire des composants plus simples, plus modulaires et plus performants. Ils permettent de séparer la logique de rendu de la logique de gestion d'état et offrent une meilleure réutilisabilité et une meilleure composabilité des composants.



