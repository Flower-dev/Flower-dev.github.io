<!-- ---
layout: post
title:  "La différence entre une fonction synchrone et une fonction asynchrone en JavaScript"
date:   2022-07-18 18:05:55 +0300
image:  05.jpg
tags:   JavaScript
--- -->

Par défaut, une fonction JavaScript est une fonction dite **synchrone**. C'est à dire que lorsque cette fonction est appelée : 
- cette dernière exécute **immédiatement** l'intégralité des instructions qui la compose avant de retourner une valeur. 
- le reste du programme attend la fin de l'excution de la fonction pour s'exécuter à son tour. 

Le navigateur exécute donc le programme ligne après ligne dans l'ordre dans lequel elles ont été écrites. Pour chaque ligne, le moteur attend que la ligne ait été exécutée avant de passer à la prochaine et ainsi de suite. En effet, chaque ligne dépend du travail exécuté dans les lignes précédentes.


Exemple d'une fonction synchrone : 

```JS
function presentation() {
    return 'Hello world';
};

const hello = presentation();

console.log('valeur retournée :', hello)
// le retour de la console est le suivant : Hello world
```

Dans l'exemple ci-dessus, `presentation()` est une fonction synchrone, car l'instruction qui l'appelle (`hello`) doit attendre que la fonction ait renvoyé sa valeur de retour avant de pouvoir se terminer.

Comment ça fonctionne lorsque l'on a plusieurs fonctions synchrones à la suite ? 

Prenons un exemple simple, lorsque l'on écrit plusieurs `console.log` à la suite, ces derniers s'exécuteront de manière séquentielle (l'un après l'autre) et toujours dans le même ordre.

```JS
// prenons ces trois console.log
console.log('A');
console.log(3);
console.log(true);

// dans la console on aura en retour A puis 3 puis true, systématiquement affichés dans cet ordre.
```
Faites le test pour vous en rendre compte par vous même 😉

Les fonctions synchrones c'est bien beau, ça attend son tour pour s'exécuter mais c'est pas toujours partique partique ... 

Pour exécuter plusieurs opérations en parallèle le tout sans bloquer le reste de l'exécution du programme on utilise les fonctions dites **asynchrones**. Cela permet à un programme de démarrer une tâche à l'exécution potentiellement longue et, au lieu d'avoir à attendre la fin de la tâche, de pouvoir continuer à réagir aux autres évènements pendant l'exécution de cette tâche. Une fois la tâche terminée, le programme en reçoit le résultat.

`setTimeout()`  est la fonction la plus répandue lorsque l'on veut exécuter du code asynchrone sans bloquer le fil d'exécution en cours. Cette fonction prend 2 paramètres :

- Le premier paramètre est la fonction à exécuter de manière asynchrone (qui sera donc ajoutée à la file d'attente de l'event loop) ;
- Le deuxième paramètre est le délai, en millisecondes, avant d'exécuter cette fonction.

Exemple : 

```JS
setTimeout(() => console.log('test setTimeout'), 5000);
console.log('Hello world')
```

Dans l'exemple ci-dessous, le texte `Hello world` s'affichera avant `test setTimeout` qui lui s'affichera 5 secondes après.


Vous avez surement dû remarquer que lorsque l'on exécute la fonction `fetch()` lors d'une requête HTTP, cette dernière ne bloque pas l'exécution du code.
La fonction `fetch()` tout comme la fonction `axios()`sont deux exemples de fonctions asynchrones car il n'est pas nécessaire d'attendre que la requête soit envoyée et que l'on ait reçu la réponse à cette dernière avant d'exécuter la suite du code.


```JS
fetch(url)
  .then(function(data) {
        // ajouter le contenu de la fonction
    })
  })
  .catch(function(error) {
        // ajouter le contenu de la fonction
  });
```
Décortiquons un peu le code ci-dessus. La méthode fetch() retourne une promesse ou Promise en anglais. 

Si la promesse renvoyée est resolve, ou résolue, cela signifie que la fonction dans la méthode `then()` a bien été exécutée. 
Dans le cas où une erreur se produirait pour une raison ou autre, la fonction renverra une promesse reject, rejetée en français. Lors d'une erreur c'est le code se trouvant dans la méthode `catch()` qui sera exécuté.

Pour résumer, une promesse est une sorte de callback générique pour le retours de fonctions asynchrones. C'est l'équivalent d'une fonction avec seulement deux possibilité de sortie : Le succès (resolve), ou l'échec (reject).