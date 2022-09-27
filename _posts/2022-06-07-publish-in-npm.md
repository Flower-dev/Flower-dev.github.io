---
layout: post
title:  "Publier sur NPM"
date:   2022-06-07 18:05:55 +0300
image:  07.jpg
tags:   NPM
---

Envie de créer un composant réutilisable en React JS et de le publier sur NPM ? Je vous explique comment faire tout cela 😉








# Publier un module sur la plateforme NPM 
Afin de publier un module sur NPM, il est nécessaire d'avoir un compte NPM, si vous n'en avez pas, vous pouvez vous en créer un directement sur la [plateforme](https://www.npmjs.com).


Une fois votre compte créé, vous devez vous y connecter à partir de votre terminal en tapant la commande suivante : 


```powershell
$ npm login 
```


N'oubliez pas de rentrer votre nom d'utilisateur et votre mot de passe ainsi que le code OPT qui vous sera envoyé par mail 😉 Vous devriez voir ce message s'afficher sur votre terminal.


```powershell
Logged in as [username] on https://registry.npmjs.org/.
```

Lancer la commande 


```powershell
$ npm init
```


afin de mettre à jour votre manifeste avec l'ensemble des informations nécessaires au bon fonctionnement de votre module. Enfin, lancer la commande suivante dans votre terminal


```powershell
$ npm publish
```


afin de publier votre composant sur NPM 

⚠️  Attention, le nom de votre composant doit être unique et le registre NPM est sensible à la casse, si jamais votre paquet rentre en conflit avec un paquet déjà existant, votre terminal vous renverra l'erreur suivante :


```powershell
403 Forbidden - PUT https://registry.npmjs.org/[package] - You do not have permission to publish "[package]". Are you logged in as the correct user?
```


Une fois publié sur NPM, votre module est prêt à être utilisé !!!


