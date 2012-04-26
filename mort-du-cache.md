# MongoDB, Mustache Can we escape caching ?

Retour d'expérience à Fotopedia.

## Fotopedia

7M de visites / mois

Il s'occupe de la BDD. 7,8 personnes chez fotopedia, donc il fait un peu
de tout.

Retranscrit son histoire sur une période de 3 ans.

3% du trafic tombe sur la page web de Paris.

## Naivement

Framework qui fait des vues : Rails + MYSQL.
Pour stocker les données de wikipedia MongoDB

Temps d'archivage des pages : ~10s

Temps: latence de transport (max 50ms), l'application.

Hypothèse, il faut que l'application réponde en moins de 50ms

## Varnish

Varnish est orienté toolkit. On écrit du code

Page en cache < 0.1ms

Objectif atteint mais avec des problèmes.

Mais tout ne peux pas être caché.

Des solutions : Requetes ajax, ou Edge Side Include.

On va cacher au grosse partie de la page, mais une ou plusieurs requêtes vont partir
(côte serveur).

## Invalidation

Déclencher une série de requêtes d'invalidation via un bus.
Difficile à mettre en oeuvre, encore plus difficile à tester.

Sur la page en cache, les parties cachées dépendent de plusieurs
critères.

Soit on n'invalide pas, mais les utilisateurs sont frustrés car ils ne
voient pas les changements.

Sinon on fait l'inverse, on invalide tout le temps le cache. (pc de
cache warming)

## Les fragments

On se retrouve avec 100 fragments sur une page

Avec Varnish les requêtes ESI sont faîtes en série -> Nginx en plus de
Varnish.

## Des fragments en JSON

On veut faire la même chose que les fragments HTML mais pour du JSON.
Ils l'ont codé.

Rajout de Lackr.


## Formatage côte client et serveur

ERB d'un côté et manipulation du DOM de l'autre.
Assez chiant à maintenir.

-> Mustache

## MongoDB

NoSQL Document Oriented + index.

Dans MongoDB quand tu fais une écriture, puis une lecture tout de suite
après, tu es certain de lire la même donnée.


Slow start 2009, new features in MongoDB 2010, die MySQL 2011

## Cache is not a silver bullet

On s'en fou que le backend soit lent, tant que l'on a du cache.
C'est mal .....

Cache warming, au début il n'y a pas de cache.
Invalidations massives pendant les migrations

Cache poisoning, on met dans le cache une donnée provenant d'un slave.
Mais malheuresement le slave n'était pas à jours.

L'empilement des caches, c'est encore plus le mal.

## Mais pourquoi utiliser du cache ?

* La base de données est lente.
* Le backend est lent (RoR)

## Pourquoi le datastore en lent

Les données ne sont pas en RAM
Les locks sur les conversations longues (jusqu'au commit)
La donnée est étalée il faut faire des JOIN.

## Stack de demain

On évite le cache JSON et on va taper dans MongoDB directement


