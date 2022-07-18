---
layout: post
title:  "La différence entre une fonction synchrone et une fonction asynchrone en JavaScript"
date:   2022-07-18 18:05:55 +0300
image:  05.jpg
tags:   JavaScript
---


# Comprendre la différence entre une fonction synchrone et une fonction asynchrone en JavaScript

Par défaut, une fonction JavaScript est une fonction dite **synchrone**. C'est à dire que lorsque cette fonction est appelée : 
- cette dernière exécute **immédiatement** l'intégralité des instructions qui la compose avant de retourner une valeur. 
- le reste du programme attend la fin de l'excution de la fonction pour s'exécuter à son tour. 

Prenons un exemple simple, lorsque l'on écrit plusieurs `console.log` à la suite, ces derniers s'exécuteront de manière séquentielle (l'un après l'autre) et toujours dans le même ordre.

```JS
// prenons ces trois console.log
console.log('A');
console.log(3);
console.log(true);

// dans la console on aura en retour A puis 3 puis true, systématiquement affichés dans cet ordre.
```
Faites le test pour vous en rendre compte par vous même 😉

